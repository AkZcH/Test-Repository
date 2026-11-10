
I was reading the Docker docs when something bothered me. Every container had its own IP address, but they all ran on one laptop, shared one kernel, and sat behind one Wi-Fi connection. An IP address is supposed to identify a device on a network. So how can one device have a dozen of them? Shouldn't containers share the host's IP and differ only by port?

This post is the story of how I answered that, not by reading more, but by poking at my own machine until I could see every hop.
## The Assumption That Broke

My mental model was simple: one machine, one IP, many ports. That model is correct for processes. Two programs on the same host both listening on `0.0.0.0` must pick different ports.

But a container is not just a process. The kernel can give a process group its own private view of the system, and the network is one of the things it can virtualize.

## Network Namespaces: A Private Network Stack

A Linux network namespace gives a process its own interfaces, its own IP addresses, its own routing table, and its own firewall rules. When Docker starts a container, it creates a new network namespace for it. Inside, the container sees only its own network stack.

I confirmed this by looking at the network devices inside a running nginx container:

```bash
docker run -d --name web1 nginx
docker exec web1 cat /proc/net/dev
```

The output listed only `lo` and a single `eth0`. None of my host's interfaces were visible. That is the namespace doing its job.

```bash
docker network inspect bridge
```

This showed `web1` with the address `172.17.0.2/16`, on a subnet whose gateway is `172.17.0.1`. A completely different private network from my Wi-Fi.

## The Missing Bridge

Docker creates a virtual switch called `docker0` on the host. Each container is plugged into it, much like a physical device plugged into a physical switch, and gets its own IP the same way any LAN device would.

So I ran `ip addr` on my host to look for `docker0`. It was not there.

The clue was in the details. My `eth0` had a Hyper-V MAC prefix (`00:15:5d`), and `docker info` reported `Operating System: Docker Desktop`. I was on WSL2 with Docker Desktop, so the Docker daemon was not running in my Ubuntu shell's network namespace at all. My `docker` command was only a client talking to a daemon elsewhere.

To look from the daemon's side, I started a container that joins the host network namespace of the Docker VM:

```bash
docker run --rm --net=host alpine ip addr
```

Now `docker0` appeared, with address `172.17.0.1/16`, along with a pair of other bridges created by earlier Compose projects.

## Following the Virtual Cable

A container reaches the bridge through a veth pair, a virtual cable with one end inside the container namespace and the other end plugged into the bridge. Each end records the index of the other.

```bash
docker exec web1 cat /sys/class/net/eth0/ifindex   # 2
docker exec web1 cat /sys/class/net/eth0/iflink    # 8
```

The container's `eth0` is interface 2 in its own namespace, and its cable ends at interface 8 somewhere else. In the host-namespace output, interface 8 was:

```text
8: vetha6fb289@services1: <BROADCAST,MULTICAST,UP,LOWER_UP> ... master docker0 state UP
```

`master docker0` means this end is a port on the bridge. It has no IPv4 address because it is just a plug. The IP `172.17.0.2` lives on the container's end of the cable.

This also explains why my Ubuntu shell could not find it. Interface indexes are only meaningful inside one namespace.

## Getting Out: NAT

A private address like `172.17.0.2` means nothing on the internet. So how do packets leave and come back? The answer is source NAT. Docker installs a rule per bridge network:

```bash
docker run --rm --net=host --privileged alpine \
  sh -c "apk add -q iptables && iptables -t nat -S POSTROUTING"
```

```text
-A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE
```

Read it as: if a packet comes from the container subnet and is leaving through any interface other than `docker0`, rewrite its source address to the outgoing interface's address. This is the same trick a home router uses to let your phone and laptop share one public IP.

## Watching It Work

I wanted to see the rule fire, so I printed the packet counters, sent traffic from a container, and printed them again:

```bash
docker run --rm --net=host --privileged alpine \
  sh -c "apk add -q iptables && iptables -t nat -L POSTROUTING -v -n"

docker run --rm alpine ping -c 3 8.8.8.8
```

The ping succeeded, and its TTL came back as 63 instead of 64. Something decremented it once, which means a router hop sat between the container and the internet. The container's traffic is routed, not merely switched.

Then the counter for the `172.17.0.0/16` MASQUERADE rule showed:

```text
1    84 MASQUERADE  all  --  *  !docker0  172.17.0.0/16  0.0.0.0/0
```

One packet, 84 bytes, even though I sent three pings. The numbers make sense once you know how NAT works. The `nat` table is consulted only for the first packet of a new connection. Connection tracking then remembers the translation and applies it to the remaining packets, and to the replies in reverse, without walking the rules again. The 84 bytes is one packet: 20 bytes of IP header, 8 bytes of ICMP header, and 56 bytes of data.

## The Full Path

Here is the path I verified, hop by hop:

```text
web1 eth0 (172.17.0.2)
  -> veth pair (container index 2 <-> VM index 8)
  -> docker0 bridge (172.17.0.1)
  -> VM eth0, after MASQUERADE
  -> Windows -> Wi-Fi
```

The last hop, where Windows translates addresses again for the Docker VM, is something I reasoned about but did not observe directly.

## What I Did Not Verify

A few things in the output I only partly understand, so I am flagging them rather than guessing:

- The `src-type LOCAL` MASQUERADE rules. They seem related to host-to-container traffic, but I have not checked the exact purpose against the Docker docs.
- The `@services1` suffix on the veth names. It differs from the usual `@ifN` form and is probably a Docker Desktop detail.

## Takeaways

1. A container has its own IP because it has its own network namespace, a private network stack.
2. It connects to the outside through a veth pair plugged into a bridge, which behaves like a virtual switch.
3. Traffic leaves through NAT, with a rule that rewrites the source address once per connection.
4. If you use Docker Desktop on Windows, your shell may not live in the same network namespace as the daemon. Run `docker run --net=host` to look from the daemon's side.

My original intuition, one device and one IP, was not stupid. It just described the network the host sees, not the one each container sees.