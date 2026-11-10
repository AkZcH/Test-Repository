Built around your 10 quiz topics, then extended with everything else a FAANG SRE loop covers. Each section has concepts, resources with links, a lab, and questions to drill.

URLs are written from memory of stable, well-known pages. If one has moved, search the title.

---

## 1. Know the target

Titles differ by company, and the loop differs with them:

|Company|Common title|Flavor|
|---|---|---|
|Google|Site Reliability Engineer (SWE or SysEng track)|Heavy on Linux internals, networking, large system design, troubleshooting, coding|
|Meta|Production Engineer|Similar mix, strong on Linux, distributed systems, and coding|
|Amazon|Systems Development Engineer, SRE roles|Leadership Principles behavioral round is huge, plus Linux and troubleshooting|
|Microsoft, Apple, Netflix|SRE, Production, Reliability titles|Varies, usually Linux, cloud, debugging, coding|

Typical loop: coding (1 to 2 rounds), Linux and systems fundamentals, troubleshooting scenario, large system design (often "non-abstract"), and behavioral. Check the specific job description once you have one and tell me, so I can reweight.

---

## 2. How your 10 quiz topics map to SRE

|Priority|Quiz topic|Why|
|---|---|---|
|Core|Q4 Linux df/du, Q2 K8s graceful shutdown, Q10 BGP/BFD and failure detection, Q3 Terraform state recovery|Classic SRE scenarios with exact mechanisms|
|Core|Q6 GitOps and controller conflicts, Q8 OIDC and workload identity|Delivery pipelines and security, constant SRE themes|
|Strong|Q7 Raft and quorum, Q5 eBPF and service meshes|Distributed systems depth that separates candidates|
|Light|Q1 GPU training, Q9 vector databases|Only matters if the team supports ML infra|

For topic-level detail on the quiz questions, see `sre-interview-study-resources.md`. This guide focuses on the SRE-specific material around them.

---

## 3. SRE principles and vocabulary (must be fluent)

**Core concepts**

- SLI, SLO, SLA, error budgets, and how budgets drive release decisions
- Burn-rate alerting (multi-window, multi-burn-rate)
- Toil and how to eliminate it
- Blameless postmortems
- Four golden signals, RED, USE methods
- Capacity planning, load testing, graceful degradation
- Cascading failures, overload, load shedding, retries with backoff and jitter

**Resources**

- Google SRE Book (free): https://sre.google/sre-book/table-of-contents/
- Google SRE Workbook (free): https://sre.google/workbook/table-of-contents/
- Implementing SLOs: https://sre.google/workbook/implementing-slos/
- Alerting on SLOs: https://sre.google/workbook/alerting-on-slos/
- Monitoring distributed systems (golden signals): https://sre.google/sre-book/monitoring-distributed-systems/
- Postmortem culture: https://sre.google/sre-book/postmortem-culture/
- Addressing cascading failures: https://sre.google/sre-book/addressing-cascading-failures/
- Handling overload: https://sre.google/sre-book/handling-overload/
- AWS Builders' Library, timeouts, retries and backoff with jitter: https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/
- AWS Builders' Library, load shedding: https://aws.amazon.com/builders-library/using-load-shedding-to-avoid-overload/
- Metastable failures paper (HotOS 2021): search the title

**Lab:** Pick a small service, define two SLIs and an SLO, implement multi-window burn-rate alerts in Prometheus, then inject errors and watch the alerts fire at the right speed.

**Be ready to answer**

- A service has a 99.9% SLO over 30 days. How much downtime is the budget? (About 43 minutes.) What does a 14.4x burn rate over 1 hour mean?
- Why do retries cause outages, and what stops them?
- Your error budget is exhausted. What changes and who decides?

---

## 4. Linux and operating system internals (highest value)

**Core concepts**

- Processes, threads, signals, fork/exec, zombies, namespaces, cgroups
- Virtual memory, page cache, swap, OOM killer, memory pressure
- File systems, inodes, file descriptors, deleted-but-open files (Quiz Q4)
- Scheduling, load average vs CPU utilization, I/O wait
- Syscalls, `strace`, `perf`, `ftrace`, eBPF tools
- Boot process, systemd, init, journald
- Performance method: USE, 60-second checklist

**Resources**

- Brendan Gregg, Linux performance: https://www.brendangregg.com/linuxperf.html
- USE method: https://www.brendangregg.com/usemethod.html
- Netflix, "Linux Performance Analysis in 60,000 Milliseconds": https://netflixtechblog.com/linux-performance-analysis-in-60-000-milliseconds-accc10403c55
- OSTEP (free OS book): https://pages.cs.wisc.edu/~remzi/OSTEP/
- Julia Evans zines and blog: https://wizardzines.com/ and https://jvns.ca
- man7.org Linux man pages: https://man7.org/linux/man-pages/
- _The Linux Programming Interface_, Michael Kerrisk (book)
- _Systems Performance_, Brendan Gregg (book)

**Lab:** On your own server, reproduce five failures and diagnose each with only CLI tools: disk full from deleted-open file, inode exhaustion, memory leak leading to OOM kill, CPU steal or throttling in a cgroup, and a process stuck in D state on I/O.

**Be ready to answer**

- What happens, step by step, when you run `ls` in a shell?
- Load average is 40 but CPU is idle. Why? (Processes blocked on I/O in uninterruptible sleep count toward load.)
- A container is OOM-killed but the host has free memory. Explain cgroup limits.
- What is the difference between `kill -15`, `-9`, and why can `-9` not be caught?

---

## 5. Networking (second highest value)

**Core concepts**

- "What happens when you type a URL": DNS, TCP handshake, TLS, HTTP/1.1 vs 2 vs 3, load balancers, response
- TCP: congestion control, retransmits, TIME_WAIT, SYN backlog, keepalives, conntrack
- DNS: resolution path, TTLs, caching, failure modes
- L4 vs L7 load balancing, health checks, connection draining
- TLS: certificates, chain validation, expiry outages, mTLS
- Tools: `tcpdump`, `ss`, `dig`, `curl -v`, `mtr`, `traceroute`
- BGP and failure detection (Quiz Q10): hold timers vs BFD
- Kubernetes networking: Services, kube-proxy, CNI, ingress (Quiz Q2, Q5)

**Resources**

- High Performance Browser Networking (free): https://hpbn.co/
- Beej's Guide to Network Programming: https://beej.us/guide/bgnet/
- How DNS works (comic): https://howdns.works/
- Cloudflare blog networking and incident postmortems: https://blog.cloudflare.com/
- _Computer Networking: A Top-Down Approach_, Kurose and Ross (book)
- BFD RFC 5880: https://datatracker.ietf.org/doc/html/rfc5880
- AWS Direct Connect docs: https://docs.aws.amazon.com/directconnect/

**Lab:** Capture a full HTTPS request with `tcpdump` and Wireshark, identify each phase. Then simulate packet loss and latency with `tc netem` and observe TCP behavior and application timeouts.

**Be ready to answer**

- Users in one region see timeouts, others fine. Walk through diagnosis.
- Why do you see many connections in TIME_WAIT, and is it a problem?
- A cert expired at 3 AM and caused an outage. What controls prevent a repeat?

---

## 6. Kubernetes and containers

**Core concepts**

- Control plane components, controllers and reconciliation, etcd
- Pod lifecycle, probes, resource requests and limits, QoS classes, eviction
- Graceful shutdown race between SIGTERM and endpoint removal (Quiz Q2)
- Scheduling, taints, affinity, PodDisruptionBudgets, HPA and VPA
- Networking and storage (CNI, CSI), RBAC, admission control
- Debugging: `describe`, events, logs, ephemeral containers, `kubectl debug`
- Container internals: namespaces, cgroups, overlay filesystems

**Resources**

- Kubernetes docs: https://kubernetes.io/docs/home/
- Pod lifecycle: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
- Learnk8s graceful shutdown: https://learnk8s.io/graceful-shutdown
- Kubernetes the Hard Way: https://github.com/kelseyhightower/kubernetes-the-hard-way
- Kubernetes Failure Stories: https://k8s.af/
- _Kubernetes in Action_, Marko Luksa (book)
- Container internals by hand: search "Liz Rice containers from scratch"

**Lab:** Build a container from scratch with `unshare` and cgroups. Then run a rolling deploy with and without `preStop` under load and count dropped requests.

**Be ready to answer**

- Pods are Pending. List the causes and how you tell them apart.
- CrashLoopBackOff versus OOMKilled versus Evicted: differences and fixes.
- What happens when etcd loses quorum?

---

## 7. Infrastructure as code, CI/CD, and release engineering

**Core concepts**

- Terraform state, locking, drift, import, recovery from crashed applies (Quiz Q3)
- Module design, blast radius, state splitting, environments
- GitOps and controller conflicts, `ignoreDifferences`, self-heal (Quiz Q6)
- Canary, blue-green, progressive delivery, automated rollback
- Pipeline security: OIDC federation, short-lived credentials, `sub` claims (Quiz Q8)
- Secrets management, least privilege, supply chain basics

**Resources**

- Terraform state: https://developer.hashicorp.com/terraform/language/state
- Terraform import: https://developer.hashicorp.com/terraform/language/import
- OpenGitOps: https://opengitops.dev/
- ArgoCD docs: https://argo-cd.readthedocs.io/en/stable/
- Argo Rollouts: https://argoproj.github.io/argo-rollouts/
- Google SRE, release engineering: https://sre.google/sre-book/release-engineering/
- Google SRE Workbook, canarying releases: https://sre.google/workbook/canarying-releases/
- GitHub OIDC with AWS: https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services
- _Terraform: Up and Running_, Yevgeniy Brikman (book)

**Lab:** Crash a Terraform apply and recover via import. Set up GitHub OIDC to AWS and deliberately break the `sub` claim three ways.

**Be ready to answer**

- Design a safe rollout pipeline for a service used by 100M users.
- A bad config was pushed globally in minutes. How do you prevent that class of outage? (Staged rollout, config validation, kill switches.)
- See the honest caveats on Q6 and Q8 in the earlier guide.

---

## 8. Observability

**Core concepts**

- Metrics, logs, traces and when each helps
- Cardinality, histogram vs summary, percentiles and why averages lie
- Alert design: symptom-based over cause-based, actionable, low noise
- Tracing with OpenTelemetry, context propagation, sampling
- Dashboards that support diagnosis, not decoration

**Resources**

- Prometheus docs: https://prometheus.io/docs/
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Google SRE, monitoring chapter (above)
- _Observability Engineering_, Majors, Fong-Jones, Miranda (book)
- Brendan Gregg, observability tools: https://www.brendangregg.com/linuxperf.html

**Lab:** Instrument a small service with Prometheus metrics and OpenTelemetry traces, build a dashboard, and add one good alert with a runbook.

**Be ready to answer**

- p99 latency doubled but CPU is flat. How do you investigate?
- How do high-cardinality labels break Prometheus?

---

## 9. Distributed systems and data

**Core concepts**

- Consensus, quorum, leader election, replication lag (Quiz Q7)
- CAP, PACELC, linearizability vs serializability, eventual consistency
- Partitioning, sharding, hot keys, consistent hashing
- Caching: invalidation, stampedes, TTL jitter
- Databases: indexes, replication, failover, backups and restore testing
- Queues and idempotency, exactly-once myths

**Resources**

- _Designing Data-Intensive Applications_, Kleppmann (book)
- Raft paper: https://raft.github.io/raft.pdf
- MIT 6.5840: https://pdos.csail.mit.edu/6.824/
- Jepsen: https://jepsen.io/consistency
- "Notes on Distributed Systems for Young Bloods": https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/
- Use the Index, Luke: https://use-the-index-luke.com/
- AWS Builders' Library: https://aws.amazon.com/builders-library/

**Be ready to answer**

- Primary database fails over. What can go wrong with split brain and replication lag?
- A cache stampede takes down the database. Prevent it three ways.
- Backups exist. How do you know restores work?

---

## 10. Troubleshooting round and incident response

This round is often the deciding one. Interviewers give a vague symptom and watch your method.

**A structure that works**

1. Clarify the symptom, scope, and timeline. What changed?
2. Mitigate first if users are hurting (rollback, drain, failover), diagnose second.
3. Form hypotheses, rank by likelihood and cost to test, test the cheapest first.
4. Narrate your reasoning out loud. State what each result rules in or out.
5. Close with the fix, the prevention, and the follow-up actions.

**Resources**

- Google SRE, managing incidents: https://sre.google/sre-book/managing-incidents/
- PagerDuty incident response docs: https://response.pagerduty.com/
- Collection of real postmortems: https://github.com/danluu/post-mortems
- DevOps exercises and interview questions: https://github.com/bregman-arie/devops-exercises
- Awesome SRE: https://github.com/dastergon/awesome-sre

**Drill scenarios**

- Website is slow for some users.
- Disk is full but `du` disagrees (Quiz Q4).
- 502s only during deploys (Quiz Q2).
- Outbound traffic fails for minutes after a link failure (Quiz Q10).
- A service's memory climbs for hours then it restarts.
- A deploy succeeded but error rate rose slowly over an hour.

Practice by having a friend or me play the interviewer with a hidden cause. Ask me and I will run it.

---

## 11. Large system design, SRE style

Google calls this non-abstract large system design (NALSD). You are expected to do real arithmetic: QPS, storage, bandwidth, number of machines, failure domains, and cost.

**Resources**

- SRE Workbook, NALSD chapter: https://sre.google/workbook/non-abstract-design/
- Google SRE, load balancing in the datacenter: https://sre.google/sre-book/load-balancing-datacenter/
- AWS Well-Architected, reliability pillar: https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html
- Principles of chaos engineering: https://principlesofchaos.org/

**Practice prompts**

1. Design a globally available URL shortener with a 99.99% SLO.
2. Design a log ingestion and search system for 1M events per second.
3. Design a rate limiter and an overload protection layer for an API tier.
4. Design a multi-region failover for a stateful service.
5. Design a metrics and alerting platform for 10K services.

For each: state SLOs, do capacity math, name failure domains, and say what happens when each component dies.

---

## 12. Coding for SRE loops

- Expect standard data structures and algorithms rounds, plus practical scripting: parse logs, aggregate metrics, implement a rate limiter, LRU cache, or a retry helper.
- You already have strong competitive programming credentials, so the risk is not raw ability. It is explaining clearly, writing clean code under time pressure, and handling edge cases.
- Pick one language (Python or Go) for interviews and be fluent in it. Practice with a timer and speak while coding.
- Resources: https://neetcode.io for pattern review, and your own LeetCode history.

---

## 13. Behavioral

- Prepare 6 to 8 stories with numbers: an incident you handled, a disagreement, a mistake you owned, a time you reduced toil, a time you learned something fast, a project you drove with ambiguity.
- Amazon: map stories to Leadership Principles. Google: collaboration, ambiguity, and humility. Meta: impact and moving fast with care.
- As an intern in an infra team, your best stories come from real incidents or automation you built. Collect the numbers now (time saved, errors reduced, latency improved).

---

## 14. Using your own projects

- **Sysrift (ptrace syscall recorder):** This is your strongest SRE-flavored project. It proves Linux internals depth. Be ready to explain ptrace, syscall tracing, nondeterminism in replay, and what you learned that applies to debugging production.
- **Weir (distributed rate limiter):** Ties into overload, load shedding, and observability. Benchmark it and be ready to discuss tail latency and failure modes.
- **Ferrux (write-ahead log):** Durability, fsync semantics, crash recovery. Natural bridge to Raft.
- **Your server at work:** Use it as a failure-injection lab (Section 4 and 5 labs), and write up each failure and diagnosis as a short post. Those write-ups are interview material.

Direct feedback: the most convincing SRE signal is evidence you have operated something. Run a small service with real SLOs, alerts, and a postmortem for a failure you caused on purpose. That beats another feature project.

---

## 15. 10-week plan

|Weeks|Focus|Deliverable|
|---|---|---|
|1 to 2|Linux internals and performance method|Five reproduced failures with written diagnoses|
|3|Networking and TCP/DNS/TLS|Annotated packet capture, netem experiments|
|4|Kubernetes depth and containers from scratch|Graceful shutdown demo, container-from-scratch repo|
|5|SRE principles, SLOs, burn-rate alerts, observability|Working SLO and alerting setup with runbook|
|6|IaC, CI/CD, GitOps, OIDC|Terraform crash recovery and OIDC demo|
|7|Distributed systems, Raft, databases|Raft lab progress, cache stampede experiment|
|8|NALSD and system design practice|Four timed designs with capacity math|
|9|Troubleshooting mocks (3 per week)|Recorded or peer-reviewed sessions|
|10|Full mock loops, behavioral stories, coding refresh|Two full-length mock loops|

Throughout: 3 to 5 coding problems per week, and one 60-second spoken explanation per day of a concept from this guide.

---

## 16. Rapid-fire questions to drill

- What is the difference between a process and a thread, and what do they share?
- Explain load average. Explain why it can mislead.
- What happens during a TCP three-way handshake, and what if the final ACK is lost?
- Why is `kill -9` sometimes ineffective? (Uninterruptible sleep.)
- How does a rolling update avoid dropping requests?
- What is an error budget policy, and what happens when it is exhausted?
- Explain quorum and why 2 of 3 survives one failure but 2 of 4 does not improve it.
- What is the difference between liveness and readiness, and what goes wrong when you mix them up?
- How do you detect and handle a retry storm?
- What would you check first if every host in a region suddenly became slow?