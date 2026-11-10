### Akshat Chauhan | Master Resource Library

> **North Star:** Become an exceptional infrastructure engineer first, document the work second, and build an audience as a consequence of real expertise.
>
> **Target identity:** AI Infrastructure Engineer building systems that make AI fast, reliable, observable and scalable.
>
> **Primary domains:** AI Infrastructure • Cloud Engineering • Distributed Systems • SRE • Systems Programming • GPU Inference
>
> **Career target:** Strong infrastructure/AI systems role, ideally AWS or a comparable high-caliber infrastructure organization.
>
> **Public target:** Technical blog + open source + talks + YouTube/Instagram/LinkedIn presence, with 50K–100K+ relevant followers as a stretch outcome rather than the primary KPI.

---
https://chatgpt.com/c/6aeb637a-45fc-83e8-a2ba-934940207bc9
# 0. How to Use This Library

This is deliberately large. **Do not try to consume everything.**

Use this priority system:

- **P0:** Core. Do these first.
- **P1:** Important. Do these after the foundations.
- **P2:** Advanced. Use when the relevant project requires them.
- **P3:** Reference. Keep bookmarked and consult when needed.

The reference plan explicitly prioritizes five technical domains: cloud/infrastructure, distributed systems, AI infrastructure, systems programming, and reliability/observability. It also recommends six flagship projects, AWS career preparation, public technical writing, open source, speaking, and a 3-year progression from foundation to authority. See the original plan for the exact target structure. 

## The rule

**Never replace building with consuming.**

For every 5 hours of technical content, aim for at least 5 hours of implementation, debugging, benchmarking, reading source code, or operating a system.

---

# 1. The Master Learning Stack

## Layer 0: Computer fundamentals

Learn:

- C
- Computer architecture
- Memory hierarchy
- Assembly basics
- Processes
- Threads
- Virtual memory
- Filesystems
- Networking
- TCP/IP
- DNS
- HTTP
- TLS
- Linux
- Concurrency
- Performance analysis

### Primary resources

1. **Computer Systems: A Programmer's Perspective**
   - Bryant & O'Hallaron
   - Use for systems foundations, memory, linking, assembly, performance and networking.
   - https://csapp.cs.cmu.edu/

2. **Operating Systems: Three Easy Pieces**
   - Remzi & Andrea Arpaci-Dusseau
   - Free online.
   - https://pages.cs.wisc.edu/~remzi/OSTEP/

3. **MIT 6.S081 / xv6**
   - Learn operating systems by modifying an actual small kernel.
   - https://pdos.csail.mit.edu/6.S081/

4. **CMU 15-213**
   - Introduction to Computer Systems.
   - https://csd.cs.cmu.edu/15213-introduction-to-computer-systems

5. **The Linux Kernel Documentation**
   - https://docs.kernel.org/

6. **Linux man pages**
   - https://man7.org/linux/man-pages/

7. **Beej's Guide to Network Programming**
   - https://beej.us/guide/bgnet/

8. **Computer Networking: A Top-Down Approach**
   - Kurose & Ross
   - https://gaia.cs.umass.edu/kurose_ross/

9. **TCP/IP Illustrated**
   - W. Richard Stevens
   - Use as a reference for TCP/IP behavior and packet-level reasoning.

10. **The Art of Computer Programming / Computer Architecture references**
   - Use selectively. Do not attempt to read encyclopedic material linearly.

---

# 2. Linux

Linux is not optional for the target role.

## Books

### P0
- The Linux Command Line — William Shotts
- How Linux Works — Brian Ward
- The Linux Programming Interface — Michael Kerrisk

### P1
- Linux Kernel Development — Robert Love
- Understanding the Linux Kernel — Bovet & Cesati
- Systems Performance — Brendan Gregg

### P2
- BPF Performance Tools — Brendan Gregg
- Linux Observability with BPF — David Calavera and colleagues/resources around BPF observability

## Official resources

- Linux Kernel Docs: https://docs.kernel.org/
- BPF docs: https://docs.kernel.org/bpf/
- man7: https://man7.org/linux/man-pages/
- kernel source: https://git.kernel.org/
- Linux Foundation training: https://training.linuxfoundation.org/

## Commands to become fluent with

```text
ps
top
htop
pidstat
vmstat
iostat
sar
ss
ip
dig
curl
tcpdump
strace
lsof
perf
journalctl
systemctl
mount
df
du
free
dmesg
grep
awk
sed
find
xargs
tar
ssh
scp
rsync
```

## Linux debugging ladder

```text
Process?
  -> ps / top / pidstat

CPU?
  -> top / perf / mpstat

Memory?
  -> free / vmstat / smaps / perf

Disk?
  -> iostat / pidstat / df / du

Network?
  -> ss / ip / tcpdump / dig

System calls?
  -> strace

Kernel behavior?
  -> perf / ftrace / eBPF

Logs?
  -> journalctl / application logs

Distributed failure?
  -> logs + metrics + traces + packet evidence
```

---

# 3. C and Systems Programming

You do not need to become a C language specialist. You do need to understand what higher-level systems code becomes.

## Books

- The C Programming Language — Kernighan & Ritchie
- Expert C Programming — Peter van der Linden
- Effective C — Robert Seacord
- Computer Systems: A Programmer's Perspective
- The Linux Programming Interface

## Practice

- Implement a shell.
- Implement a memory allocator.
- Implement a thread pool.
- Implement a TCP server.
- Implement an HTTP server.
- Implement a process supervisor.
- Implement a simple key-value store.
- Implement a userspace tracer.

---

# 4. Go

## Goal

Make Go your default language for infrastructure services.

The reference plan recommends Go for infrastructure services, Rust for performance-sensitive systems, and Python for AI tooling/experimentation.

## Official

- A Tour of Go: https://go.dev/tour/
- Effective Go: https://go.dev/doc/effective_go
- Go specification: https://go.dev/ref/spec
- Go blog: https://go.dev/blog/
- Go by Example: https://gobyexample.com/
- Go documentation: https://go.dev/doc/

## Books

### P0
- Learning Go — Jon Bodner
- Let's Go — Alex Edwards

### P1
- Let's Go Further — Alex Edwards
- 100 Go Mistakes — Teiva Harsanyi

### P2
- Network Programming with Go
- Go in Action

## Must master

- Interfaces
- Goroutines
- Channels
- Context
- Cancellation
- Error handling
- HTTP
- gRPC
- net/http
- sync
- atomic
- mutexes
- worker pools
- backpressure
- profiling
- benchmarks
- race detector
- pprof
- testing
- fuzzing

## Go projects

1. HTTP server
2. Concurrent crawler
3. Worker pool
4. Rate limiter
5. L7 proxy
6. Distributed queue
7. KV store
8. Service discovery system
9. Metrics collector
10. Distributed scheduler

---

# 5. Rust

## Goal

Use Rust for performance-sensitive and safety-critical systems work.

## Official

- The Rust Book: https://doc.rust-lang.org/book/
- Rust By Example: https://doc.rust-lang.org/rust-by-example/
- Rust Reference: https://doc.rust-lang.org/reference/
- Rustonomicon: https://doc.rust-lang.org/nomicon/
- Async Book: https://rust-lang.github.io/async-book/
- Tokio: https://tokio.rs/
- Rust Blog: https://blog.rust-lang.org/

## Books

- Programming Rust — Jim Blandy, Jason Orendorff, Leonora Tindall
- Rust for Rustaceans — Jon Gjengset
- Zero To Production in Rust — Luca Palmieri

## Must understand

- Ownership
- Borrowing
- Lifetimes
- Traits
- Generics
- Enums
- Pattern matching
- Async
- Futures
- Tokio
- Channels
- Arc/Mutex/RwLock
- Atomics
- Unsafe Rust
- FFI
- Profiling

## Projects

- TCP server
- HTTP server
- Async proxy
- Memory pool
- Compression library
- KV store
- eBPF userspace tooling
- High-performance rate limiter

---

# 6. Networking

This is a major differentiator for infrastructure engineers.

## Books

### P0
- Computer Networking: A Top-Down Approach
- TCP/IP Illustrated, Volume 1

### P1
- Unix Network Programming — Stevens
- High Performance Browser Networking — Ilya Grigorik

## Concepts

```text
Ethernet
ARP
IP
IPv4
IPv6
CIDR
Subnetting
Routing
NAT
TCP
UDP
QUIC
TLS
DNS
HTTP/1.1
HTTP/2
HTTP/3
gRPC
WebSockets
Load balancing
Reverse proxies
Connection pooling
Keep-alive
Backpressure
Congestion control
```

## Tools

- Wireshark
- tcpdump
- ss
- iproute2
- dig
- nslookup
- curl
- netcat
- traceroute
- mtr
- iperf3
- nmap
- eBPF tools

## Practice

Build:

1. TCP echo server
2. HTTP server
3. Reverse proxy
4. L4 load balancer
5. L7 load balancer
6. Service discovery system
7. Connection pool
8. Rate limiter
9. DNS resolver
10. Network observability tool

---

# 7. Distributed Systems

This is one of the central pillars.

## Core topics

### P0

- Failure models
- Clocks
- Ordering
- Replication
- Leader election
- Consensus
- Quorums
- Consistent hashing
- Partitioning
- Sharding
- Transactions
- Consistency models
- Fault tolerance
- Distributed locking
- Distributed queues
- Service discovery
- Membership
- Gossip

### P1

- CRDTs
- Vector clocks
- Lamport clocks
- Byzantine fault tolerance
- Distributed transactions
- 2PC
- 3PC
- MVCC
- LSM trees
- WAL
- Log replication
- Geo-replication
- Multi-region systems
- RDMA
- DPDK

---

# 8. Distributed Systems Books

## P0

### Designing Data-Intensive Applications
Martin Kleppmann

Read this deeply.

https://dataintensive.net/

Focus on:

- Reliable systems
- Scalability
- Maintainability
- Replication
- Partitioning
- Transactions
- Distributed systems
- Batch processing
- Stream processing

### Database Internals
Alex Petrov

https://www.databass.dev/

Focus on:

- Storage engines
- B-trees
- LSM trees
- WAL
- Distributed databases

### Distributed Systems
Maarten van Steen & Andrew S. Tanenbaum

https://www.distributed-systems.net/

Use as a structured theoretical reference.

## P1

- Distributed Systems — George Coulouris et al.
- Designing Distributed Systems — Brendan Burns
- Understanding Distributed Systems — Roberto Vitillo
- Distributed Algorithms — Nancy Lynch
- Distributed Computing — Hagit Attiya & Jennifer Welch

## P2

- Database Reliability Engineering
- Transaction Processing — Jim Gray
- Principles of Distributed Database Systems
- Distributed Systems: Concepts and Design

---

# 9. Distributed Systems Courses

## P0: MIT 6.5840

https://pdos.csail.mit.edu/6.5840/

This should be one of your highest-priority resources.

The 2026 course covers fault tolerance, replication and consistency and includes substantial programming labs.

### Labs

- MapReduce
- Key/value service
- Raft
- Fault-tolerant KV
- Sharded KV

Use the labs as serious engineering exercises, not as something to merely watch.

The Raft lab builds a replicated state machine and later labs build KV and sharded KV services on top of it.

## P1

- Stanford distributed systems materials
- CMU distributed systems courses
- Berkeley systems courses
- Cornell distributed systems materials
- University of Washington distributed systems courses

## Project ideas from MIT-style systems work

- Distributed cache
- Fault-tolerant file service
- Geo-replicated storage
- Distributed block store
- Distributed shared memory
- Eventual-consistency system
- Distributed web cache
- High-speed RDMA service

---

# 10. Consensus

## Must know

- Raft
- Paxos
- Multi-Paxos
- Viewstamped Replication
- Zab
- Byzantine consensus basics

## Papers

### P0

**In Search of an Understandable Consensus Algorithm (Raft)**

https://raft.github.io/raft.pdf

### P0

**Paxos Made Simple**

Leslie Lamport

http://lamport.azurewebsites.net/pubs/paxos-simple.pdf

### P1

- Viewstamped Replication Revisited
- Byzantine Fault Tolerance
- PBFT
- EPaxos
- Flexible Paxos

## Practice

Implement:

```text
Leader election
Log replication
Commit index
Term handling
Persistent state
Crash recovery
Network partitions
Leader failure
Snapshotting
Membership changes
```

Then benchmark it.

---

# 11. Distributed Storage

## Systems to study

- Dynamo
- Cassandra
- Bigtable
- Spanner
- CockroachDB
- FoundationDB
- TiKV
- etcd
- RocksDB
- LevelDB
- Redis

## Papers

### Dynamo

Amazon Dynamo:
A Highly Available Key-value Store

https://www.amazon.science/publications/dynamo-amazons-highly-available-key-value-store

### Bigtable

Bigtable: A Distributed Storage System for Structured Data

https://research.google/pubs/bigtable-a-distributed-storage-system-for-structured-data/

### Spanner

Spanner: Google's Globally-Distributed Database

https://research.google/pubs/spanner-googles-globally-distributed-database/

### MapReduce

MapReduce: Simplified Data Processing on Large Clusters

https://research.google/pubs/mapreduce-simplified-data-processing-on-large-clusters/

### Borg

Large-scale cluster management at Google with Borg

https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/

### Tail at Scale

The Tail at Scale

https://research.google/pubs/the-tail-at-scale/

---

# 12. Databases

## Books

- Database Internals — Alex Petrov
- Designing Data-Intensive Applications — Martin Kleppmann
- Database System Concepts — Silberschatz, Korth, Sudarshan
- Readings in Database Systems — Red Book
- Transaction Processing — Jim Gray

## Study

### Storage

- Pages
- Records
- Buffer pools
- WAL
- B-trees
- LSM trees
- SSTables
- Bloom filters
- Compaction

### Query execution

- Parsing
- Planning
- Cost estimation
- Index selection
- Join algorithms
- Execution engines

### Transactions

- ACID
- Isolation
- Serializability
- MVCC
- Locks
- Deadlocks
- Recovery

## Source code

Read:

- PostgreSQL
- SQLite
- Redis
- RocksDB
- etcd

---

# 13. Caching

## Study

- Local caching
- Distributed caching
- Cache invalidation
- TTL
- LRU
- LFU
- ARC
- Write-through
- Write-back
- Read-through
- Cache stampede
- Cache warming
- Consistent hashing

## Resources

- Redis documentation: https://redis.io/docs/
- Memcached: https://memcached.org/
- RocksDB: https://rocksdb.org/

## Build

A distributed cache with:

- consistent hashing
- replication
- eviction
- TTL
- metrics
- failure recovery
- benchmark suite

---

# 14. Message Queues and Streaming

## Learn

- Kafka
- NATS
- RabbitMQ
- Pulsar
- Redis Streams
- SQS
- Kinesis

## Concepts

- Producer
- Consumer
- Partition
- Offset
- Consumer group
- Ordering
- Delivery semantics
- At-most-once
- At-least-once
- Exactly-once semantics
- Backpressure
- Replay
- Dead-letter queues

## Books

- Kafka: The Definitive Guide
- Designing Event-Driven Systems
- Enterprise Integration Patterns

## Official docs

- Kafka: https://kafka.apache.org/documentation/
- NATS: https://docs.nats.io/
- RabbitMQ: https://www.rabbitmq.com/docs
- Apache Pulsar: https://pulsar.apache.org/docs/
- AWS SQS: https://docs.aws.amazon.com/sqs/
- AWS Kinesis: https://docs.aws.amazon.com/kinesis/

---

# 15. AWS

AWS should be learned as an architecture platform, not a list of services.

## P0 foundation

- IAM
- EC2
- VPC
- S3
- EBS
- EFS
- RDS
- DynamoDB
- Route 53
- CloudFront
- ELB
- Auto Scaling
- CloudWatch
- CloudTrail
- KMS
- Secrets Manager

## P1

- EKS
- ECS
- Lambda
- API Gateway
- SQS
- SNS
- EventBridge
- Kinesis
- ElastiCache
- OpenSearch
- Aurora
- Bedrock
- SageMaker
- ECR
- Fargate

## P2

- Nitro architecture
- Graviton
- Inferentia
- Trainium
- Neuron
- PrivateLink
- Transit Gateway
- Direct Connect
- Route 53 Resolver
- Global Accelerator
- Multi-region architecture

## Official AWS learning

### AWS Ramp-Up Guides

https://aws.amazon.com/training/ramp-up-guides/

Use:

- Developer
- DevOps
- Solutions Architect
- Cloud Operator
- AI/ML

### AWS Skill Builder

https://skillbuilder.aws/

### AWS Documentation

https://docs.aws.amazon.com/

### AWS Architecture Center

https://aws.amazon.com/architecture/

### AWS Well-Architected

https://aws.amazon.com/architecture/well-architected/

### AWS Builders' Library

https://aws.amazon.com/builders-library/

This is extremely high-value for your target.

Read deeply:

- Timeouts, retries and backoff
- Static stability
- Avoiding overload
- Load shedding
- Leader election
- Cell-based architecture
- Automating safe operations
- Reliability patterns

---

# 16. AWS Certifications

Certifications are secondary.

Recommended order:

1. AWS Certified Solutions Architect – Associate
2. AWS Certified DevOps Engineer – Professional
3. AWS Certified Solutions Architect – Professional
4. Specialty certification only if your target role benefits from it

Do not spend months collecting badges.

Use certifications to structure learning, not substitute for engineering.

---

# 17. AWS YouTube / Video

## Channels

- AWS
- AWS Developers
- AWS Events
- AWS re:Invent
- AWS Architecture
- AWS Online Tech Talks

## Watch

- AWS re:Invent architecture talks
- Builders' Library sessions
- EKS deep dives
- networking deep dives
- serverless architecture
- AWS AI infrastructure
- Trainium/Inferentia talks

---

# 18. Kubernetes

## Official

- Documentation: https://kubernetes.io/docs/
- Tutorials: https://kubernetes.io/docs/tutorials/
- Concepts: https://kubernetes.io/docs/concepts/
- Tasks: https://kubernetes.io/docs/tasks/
- Kubernetes Blog: https://kubernetes.io/blog/
- GitHub: https://github.com/kubernetes/kubernetes

## Books

### P0
- Kubernetes Up & Running — Kelsey Hightower, Brendan Burns, Joe Beda

### P1
- Kubernetes Patterns — Bilgin Ibryam, Roland Huß
- Production Kubernetes — Josh Rosso et al.
- Kubernetes Best Practices — Brendan Burns, Eddie Villalba

## Must understand

- Pods
- Deployments
- ReplicaSets
- Services
- Ingress
- Gateway API
- ConfigMaps
- Secrets
- StatefulSets
- DaemonSets
- Jobs
- CronJobs
- Volumes
- StorageClasses
- RBAC
- NetworkPolicies
- Scheduling
- Taints/tolerations
- Affinity
- HPA
- VPA
- Cluster Autoscaler
- CRDs
- Operators
- Admission
- Control plane
- kubelet
- etcd
- CNI
- CSI

## Advanced

- Scheduler internals
- kube-proxy
- CNI internals
- container runtime
- CRI
- Gateway API
- device plugins
- GPU scheduling
- DRA
- multi-cluster
- cluster federation

---

# 19. Containers

## Docker

Official:

- https://docs.docker.com/get-started/
- https://docs.docker.com/guides/

Learn:

- namespaces
- cgroups
- images
- layers
- registries
- overlay filesystem
- networking
- volumes
- Compose
- multi-stage builds
- rootless containers
- image security

## Container internals

Study:

- Linux namespaces
- cgroups
- seccomp
- capabilities
- AppArmor
- overlayfs
- containerd
- runc
- OCI

## Projects

- Build a container runtime
- Build a tiny OCI-compatible container
- Implement process isolation
- Build a container network experiment

---

# 20. Terraform / Infrastructure as Code

## Official

- Terraform tutorials: https://developer.hashicorp.com/terraform/tutorials
- Terraform docs: https://developer.hashicorp.com/terraform/docs
- HashiCorp Learn: https://developer.hashicorp.com/tutorials

## Learn

- Providers
- Resources
- Variables
- Outputs
- Modules
- State
- Remote state
- State locking
- Workspaces
- Data sources
- Dependency graph
- Import
- Drift
- Testing
- Policy

## Build

Provision:

```text
VPC
Subnets
NAT
ALB
EKS
RDS
S3
IAM
CloudWatch
Route 53
```

Then destroy and recreate the environment from scratch.

---

# 21. Observability

## Core concepts

- Metrics
- Logs
- Traces
- Profiles
- Events
- SLI
- SLO
- SLA
- Error budgets
- Alerting
- Incident response
- Root cause analysis

## OpenTelemetry

Official docs:

https://opentelemetry.io/docs/

Learn:

- instrumentation
- spans
- traces
- metrics
- logs
- context propagation
- collectors
- exporters
- semantic conventions

## Prometheus

https://prometheus.io/docs/

Learn:

- metrics model
- PromQL
- exporters
- scraping
- recording rules
- alerting
- federation
- remote write

## Grafana

https://grafana.com/docs/

Learning hub:

https://grafana.com/docs/learning-hub/

Learn:

- dashboards
- alerts
- Loki
- Tempo
- Mimir
- Alloy
- CloudWatch integration
- Kubernetes monitoring

---

# 22. SRE

## Books

### P0

**Site Reliability Engineering**
Google

https://sre.google/sre-book/

Free online.

### P0

**The Site Reliability Workbook**

https://sre.google/workbook/

### P1

- Seeking SRE
- Implementing SRE
- Database Reliability Engineering

## Topics

- SLI
- SLO
- SLA
- Error budgets
- Toil
- Incident management
- Capacity planning
- Load testing
- Reliability engineering
- Risk management
- Change management
- Automation

## Practice

For every project:

1. Define SLIs.
2. Define SLOs.
3. Create alerts.
4. Introduce failure.
5. Observe the failure.
6. Diagnose it.
7. Recover.
8. Write a postmortem.

---

# 23. Performance Engineering

## Essential book

**Systems Performance**
Brendan Gregg

https://www.brendangregg.com/systems-performance-2nd-edition-book.html

## Advanced book

**BPF Performance Tools**
Brendan Gregg

https://www.brendangregg.com/bpf-performance-tools-book.html

## Learn

- CPU utilization
- CPU saturation
- memory bandwidth
- cache misses
- context switches
- syscalls
- I/O
- latency
- throughput
- tail latency
- lock contention
- scheduling
- NUMA

## Tools

```text
perf
bcc
bpftrace
bpftool
strace
ftrace
flamegraphs
sar
iostat
vmstat
pidstat
mpstat
perfetto
```

---

# 24. eBPF

## Official

- Linux BPF docs: https://docs.kernel.org/bpf/
- eBPF project: https://ebpf.io/
- Cilium BPF reference: https://docs.cilium.io/en/stable/bpf/

## Books/resources

- Learning eBPF — Liz Rice
- BPF Performance Tools — Brendan Gregg
- Linux Observability with BPF
- eBPF Summit talks

## Projects

1. TCP connection tracer
2. HTTP latency tracer
3. Process execution tracer
4. DNS tracer
5. File access tracer
6. syscall profiler
7. network flow monitor
8. Kubernetes network observability tool

---

# 25. AI Infrastructure

This is your differentiating specialization.

## Fundamentals

You need to understand:

- GPU architecture
- CUDA
- CUDA memory hierarchy
- kernels
- warps
- SMs
- occupancy
- memory bandwidth
- arithmetic intensity
- FLOPs
- communication overhead
- NCCL
- distributed GPU execution

---

# 26. CUDA

## Official

- CUDA documentation: https://docs.nvidia.com/cuda/
- CUDA C Programming Guide: https://docs.nvidia.com/cuda/cuda-c-programming-guide/
- NVIDIA Developer: https://developer.nvidia.com/cuda-zone
- NVIDIA DLI: https://www.nvidia.com/training/

## Courses

### P0

NVIDIA DLI:
**Getting Started with Accelerated Computing with CUDA C/C++**

### P1

- CUDA C/C++ training
- Nsight Systems
- Nsight Compute
- GPU performance optimization

## Learn

- threads
- blocks
- grids
- warps
- shared memory
- global memory
- registers
- coalescing
- occupancy
- synchronization
- atomics
- streams
- events
- CUDA graphs

---

# 27. LLM Fundamentals

## Courses

### P0

**Stanford CS336: Language Modeling from Scratch**

https://cs336.stanford.edu/

This is particularly relevant because it connects model construction with systems optimization, GPUs, parallelism, Triton and distributed execution.

### P0

**Hugging Face Course**

https://huggingface.co/learn

It covers Transformers, Datasets, Tokenizers, Accelerate and the Hub.

### P1

- Full Stack Deep Learning
- DeepLearning.AI LLM courses
- Stanford CS224N
- Stanford CS25
- MIT NLP courses

## Books

- Natural Language Processing with Transformers
- Build a Large Language Model From Scratch — Sebastian Raschka
- Designing Machine Learning Systems — Chip Huyen
- Machine Learning Systems Design — Chip Huyen
- Introduction to Machine Learning Systems — MLSysBook

---

# 28. Machine Learning Systems

## Books

### P0

**Designing Machine Learning Systems**
Chip Huyen

https://huyenchip.com/books/

### P0

**Machine Learning Systems**
MLSysBook

https://mlsysbook.ai/

The MLSysBook project is open access and includes readings, experiments, courses, labs and companion resources.

### P1

- Machine Learning Engineering — Andriy Burkov
- Machine Learning Design Patterns
- Reliable Machine Learning — Cathy Chen et al.
- Hidden Technical Debt in Machine Learning Systems

---

# 29. LLM Serving

## Core concepts

- batching
- continuous batching
- dynamic batching
- KV cache
- prefix caching
- paged attention
- quantization
- speculative decoding
- tensor parallelism
- pipeline parallelism
- expert parallelism
- model parallelism
- data parallelism
- prefill
- decode
- TTFT
- TPOT
- inter-token latency
- throughput
- latency
- GPU utilization
- memory utilization

---

# 30. vLLM

## Official

https://docs.vllm.ai/

Read:

- architecture
- serving
- performance
- batching
- KV cache
- distributed inference
- quantization
- parallelism
- profiling
- production deployment

The current vLLM documentation includes optimization and tuning guidance, including optimization levels and memory/performance tradeoffs.

## Project

Build:

**Your own mini inference server**

Start with:

```text
Python
FastAPI
PyTorch
Transformers
```

Then move toward:

```text
vLLM
Kubernetes
Prometheus
Grafana
AWS
GPU
```

Benchmark:

- requests/sec
- tokens/sec
- TTFT
- TPOT
- p50
- p95
- p99
- GPU utilization
- VRAM usage
- cost/request

---

# 31. NVIDIA Triton

## Official

- https://docs.nvidia.com/deeplearning/triton-inference-server/
- Tutorials: https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/tutorials/

Learn:

- model repository
- HTTP
- gRPC
- dynamic batching
- ensembles
- model configuration
- model analyzer
- TensorRT
- Python backend
- metrics
- Kubernetes deployment

Triton's official tutorials include PyTorch, TensorFlow, ONNX, TensorRT, vLLM and Hugging Face deployment examples.

---

# 32. KServe

## Official

https://kserve.github.io/

Learn:

- InferenceService
- autoscaling
- traffic management
- model versioning
- canary deployments
- inference graphs
- Kubernetes integration

KServe's current resource model uses Kubernetes CRDs for declarative model serving and includes InferenceService and InferenceGraph abstractions.

---

# 33. Ray

## Official

https://docs.ray.io/

Learn:

- Ray Core
- actors
- tasks
- distributed execution
- Ray Serve
- Ray Train
- Ray Data
- Ray Tune

Use Ray when the problem actually requires distributed Python execution. Do not add it because it looks impressive.

---

# 34. AI Infrastructure Papers

Read these carefully.

## P0

### vLLM / PagedAttention

**Efficient Memory Management for Large Language Model Serving with PagedAttention**

https://arxiv.org/

Understand:

- KV cache fragmentation
- paging
- memory management
- batching
- serving throughput

### FlashAttention

**FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness**

https://arxiv.org/

Understand:

- IO complexity
- SRAM/HBM
- tiling
- memory movement

### Megatron-LM

**Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism**

https://arxiv.org/

Understand:

- tensor parallelism
- pipeline parallelism
- model scaling

### DeepSpeed

Read the DeepSpeed papers and documentation.

https://www.deepspeed.ai/

### Orca

**A Distributed Serving System for Transformer-Based Generative Models**

Study iteration-level scheduling.

### DistServe

Study separation of prefill and decode.

### SGLang

Study structured generation and efficient execution.

### Speculative Decoding

Study how a smaller model can accelerate generation from a larger model.

---

# 35. AI Infrastructure Engineering Papers / Systems to Study

Create a paper folder and read:

```text
Attention Is All You Need
BERT
GPT-3
Chinchilla
FlashAttention
FlashAttention-2
FlashAttention-3
Megatron-LM
ZeRO
DeepSpeed
vLLM / PagedAttention
Orca
DistServe
SGLang
TensorRT-LLM
Alpa
GSPMD
Ray
Parameter Server
PipeDream
GPipe
Switch Transformer
Mixture-of-Experts
NCCL architecture papers
Triton-related papers
```

Do not try to memorize papers.

For each paper answer:

```text
What problem exists?
Why do existing systems fail?
What abstraction is introduced?
What is the key algorithm?
What is the bottleneck?
What tradeoff is made?
What benchmark proves the improvement?
What would break at 10x scale?
```

---

# 36. GPU / AI Infrastructure Talks

## Channels

- NVIDIA
- NVIDIA Developer
- Google Cloud Tech
- AWS
- AWS Events
- Microsoft Azure
- Meta
- OpenAI
- Anthropic
- Hugging Face
- Databricks
- Anyscale
- PyTorch
- Linux Foundation
- CNCF

## High-value event series

- NVIDIA GTC
- AWS re:Invent
- KubeCon + CloudNativeCon
- PyTorch Conference
- MLSys
- OSDI
- SOSP
- NSDI
- EuroSys
- Hot Chips
- NeurIPS Systems workshops
- MLSys conferences

---

# 37. PyTorch

## Official

https://pytorch.org/

Learn:

- tensors
- autograd
- modules
- CUDA
- distributed
- DDP
- FSDP
- compilation
- profiling
- memory management

## Learn to profile

- PyTorch Profiler
- Nsight Systems
- Nsight Compute

---

# 38. Hugging Face

## Official

- https://huggingface.co/
- https://huggingface.co/learn
- https://huggingface.co/docs

Learn:

- Transformers
- Datasets
- Tokenizers
- Accelerate
- PEFT
- Hub
- inference
- quantization

---

# 39. Kubernetes AI Stack

Eventually learn:

```text
Kubernetes
  |
  +-- NVIDIA GPU Operator
  |
  +-- Device Plugins / DRA
  |
  +-- KServe
  |
  +-- vLLM
  |
  +-- Triton
  |
  +-- Prometheus
  |
  +-- Grafana
  |
  +-- OpenTelemetry
```

Understand every component before trying to operate the entire stack.

---

# 40. AI Infrastructure on AWS

Study:

- EC2 GPU instances
- EKS GPU workloads
- ECR
- S3 model storage
- FSx
- EBS
- CloudWatch
- IAM
- VPC networking
- autoscaling
- load balancing
- Inferentia
- Trainium
- Neuron
- SageMaker
- Bedrock

## Project

Build:

**Production-style LLM inference platform on AWS**

Architecture:

```text
Client
  |
Route 53
  |
CloudFront / ALB
  |
API Gateway
  |
EKS
  |
Inference Gateway
  |
vLLM / Triton
  |
GPU Workers
  |
S3 / Model Registry

Observability:
OpenTelemetry
Prometheus
Grafana
CloudWatch
```

Measure cost and performance.

---

# 41. Your Six Flagship Projects

The reference plan recommends six flagship systems:

1. Distributed Rate Limiter
2. Distributed KV Store
3. LLM Inference Platform
4. Distributed Observability Platform
5. L7 Load Balancer
6. Cloud Chaos Engineering Platform

For every project produce:

```text
README
Architecture diagram
Design document
API specification
Source code
Unit tests
Integration tests
Load tests
Benchmark report
Failure testing
Observability dashboard
Deployment instructions
Cloud deployment
Technical article
Demo video
Postmortem
Lessons learned
Future work
```

---

# 42. Project 1: Distributed Rate Limiter

## Stack

```text
Go
Redis
gRPC
Docker
Kubernetes
Prometheus
Grafana
AWS
Terraform
```

## Features

- Token bucket
- Leaky bucket
- Fixed window
- Sliding window
- Distributed coordination
- Consistent hashing
- Redis backend
- local cache
- rate-limit headers
- fail-open/fail-closed policy
- metrics
- tracing

## Benchmark

Measure:

- throughput
- p50
- p95
- p99
- CPU
- memory
- Redis latency
- network overhead
- failure recovery

## Article ideas

- "I Built a Distributed Rate Limiter in Go"
- "Why Rate Limiting Becomes a Distributed Systems Problem"
- "What Happens When Redis Dies?"
- "Benchmarking Token Bucket vs Sliding Window"

---

# 43. Project 2: Distributed KV Store

## Implement

- WAL
- memtable
- SSTables
- compaction
- replication
- Raft
- snapshots
- sharding
- rebalancing

## Stack

Go or Rust.

## Benchmark

- read throughput
- write throughput
- tail latency
- replication overhead
- recovery time
- compaction overhead

---

# 44. Project 3: LLM Inference Platform

## Stack

```text
Python
PyTorch
vLLM
Kubernetes
AWS
Terraform
Prometheus
Grafana
OpenTelemetry
```

## Features

- model registry
- inference API
- autoscaling
- batching
- monitoring
- GPU metrics
- request tracing
- canary releases
- model versioning
- load testing

## Metrics

```text
TTFT
TPOT
tokens/sec
requests/sec
p50
p95
p99
GPU utilization
VRAM
cost/request
```

---

# 45. Project 4: Distributed Observability Platform

## Stack

```text
Go
OpenTelemetry
Prometheus
Grafana
Loki
Tempo
Kafka
Kubernetes
```

## Features

- metrics ingestion
- logs ingestion
- trace ingestion
- correlation
- service map
- dashboards
- alerting
- SLOs

---

# 46. Project 5: L7 Load Balancer

## Stack

```text
Go
Linux
TCP
HTTP
TLS
eBPF
Docker
Kubernetes
```

## Features

- HTTP routing
- host routing
- path routing
- health checks
- retries
- circuit breaking
- connection pooling
- load balancing algorithms
- TLS termination
- metrics

Algorithms:

- round robin
- least connections
- weighted round robin
- consistent hashing
- latency-aware routing

---

# 47. Project 6: Chaos Engineering Platform

## Stack

```text
Go
Kubernetes
AWS
Terraform
Prometheus
Grafana
OpenTelemetry
```

Inject:

- pod failures
- CPU pressure
- memory pressure
- network delay
- packet loss
- DNS failure
- disk pressure
- node termination
- dependency failure

Then measure:

- recovery time
- error rate
- latency
- SLO impact
- blast radius

---

# 48. Open Source

Do not start by submitting random typo fixes forever.

## Strategy

### Stage 1

- Documentation
- Reproduce bugs
- Small bug fixes
- Tests

### Stage 2

- Medium bug fixes
- Features
- Performance improvements

### Stage 3

- Maintainer relationships
- Design discussions
- Architecture contributions

## Projects to study/contribute to

- Kubernetes
- containerd
- Cilium
- Envoy
- Prometheus
- OpenTelemetry
- Grafana
- etcd
- TiKV
- RocksDB
- Redis
- vLLM
- KServe
- Ray
- PyTorch
- Triton
- Rust projects
- Go projects

Pick **one or two** initially.

---

# 49. Reading Engineering Blogs

## P0

### AWS Builders' Library
https://aws.amazon.com/builders-library/

### AWS Architecture Blog
https://aws.amazon.com/blogs/architecture/

### Google Research
https://research.google/

### Google Cloud Blog
https://cloud.google.com/blog/

### Netflix TechBlog
https://netflixtechblog.com/

### Uber Engineering
https://www.uber.com/blog/engineering/

### Cloudflare Blog
https://blog.cloudflare.com/

### Meta Engineering
https://engineering.fb.com/

### LinkedIn Engineering
https://engineering.linkedin.com/

### Discord Engineering
https://discord.com/blog

### Stripe Engineering
https://stripe.com/blog/engineering

### Slack Engineering
https://slack.engineering/

### Datadog Engineering
https://www.datadoghq.com/blog/engineering/

### GitHub Engineering
https://github.blog/engineering/

### DoorDash Engineering
https://doordash.engineering/

---

# 50. Engineering Blog Reading List

Search these blogs for:

```text
distributed systems
consensus
storage
caching
load balancing
Kubernetes
observability
GPU
inference
LLM serving
capacity planning
reliability
incident response
performance
networking
```

Do not read everything chronologically.

Use targeted searches around the system you are currently building.

---

# 51. Research Paper Sources

## Primary

- Google Research: https://research.google/
- Amazon Science: https://www.amazon.science/
- Microsoft Research: https://www.microsoft.com/en-us/research/
- Meta AI: https://ai.meta.com/research/
- NVIDIA Research: https://research.nvidia.com/
- OpenAI Research: https://openai.com/research/
- Anthropic Research: https://www.anthropic.com/research
- Stanford: https://cs.stanford.edu/
- MIT CSAIL: https://www.csail.mit.edu/
- Berkeley RISELab / systems research
- CMU Systems: https://www.cs.cmu.edu/

## Paper indexes

- arXiv: https://arxiv.org/
- Semantic Scholar: https://www.semanticscholar.org/
- Google Scholar: https://scholar.google.com/
- Papers with Code: https://paperswithcode.com/
- DBLP: https://dblp.org/
- OpenReview: https://openreview.net/

## Conferences

- OSDI
- SOSP
- NSDI
- EuroSys
- FAST
- ATC
- SIGMOD
- VLDB
- KDD
- MLSys
- NeurIPS
- ICML
- ICLR
- ASPLOS
- ISCA
- MICRO

---

# 52. Essential Distributed Systems Paper List

Read in roughly this order.

## Foundations

1. MapReduce
2. Bigtable
3. Dynamo
4. Spanner
5. Borg
6. Chubby
7. Raft
8. Paxos Made Simple
9. GFS
10. The Tail at Scale

## Storage

11. LSM-tree
12. LevelDB
13. RocksDB papers
14. Cassandra papers
15. FoundationDB papers
16. CockroachDB technical papers

## Consistency

17. CAP-related work
18. COPS
19. Bayou
20. CRDT papers
21. Spanner / TrueTime

## Performance

22. The Tail at Scale
23. Dapper
24. Facebook TAO
25. Memcache papers

---

# 53. Essential SRE / Reliability Papers and Resources

Read:

- Google's SRE Book
- Google's SRE Workbook
- The Tail at Scale
- Dapper
- Borg
- Chubby
- Site Reliability Engineering literature
- Chaos Engineering principles
- AWS Builders' Library

Study real incidents from:

- Google
- AWS
- Cloudflare
- GitHub
- Meta
- Netflix
- Slack
- Discord

The goal is not gossip about outages.

For every incident ask:

```text
What failed?
Why did it fail?
Why wasn't it detected earlier?
Why did the blast radius become large?
What control failed?
What would have prevented it?
What would have reduced the blast radius?
```

---

# 54. System Design

## Books

- System Design Interview — Alex Xu
- System Design Interview Vol. 2 — Alex Xu
- Designing Data-Intensive Applications
- Understanding Distributed Systems

Use Alex Xu for interview structure.

Use DDIA for actual depth.

Do not confuse the two.

## Topics

- URL shortener
- notification system
- chat system
- rate limiter
- news feed
- search
- file storage
- video streaming
- metrics platform
- logging platform
- distributed scheduler
- ride sharing
- payment system
- inference platform
- feature store
- model registry

---

# 55. System Design Practice Method

For every design:

```text
1. Requirements
2. Scale estimation
3. API
4. Data model
5. High-level architecture
6. Storage
7. Caching
8. Partitioning
9. Replication
10. Consistency
11. Failure modes
12. Observability
13. Security
14. Cost
15. Bottlenecks
16. Scaling strategy
```

Then ask:

> What breaks first?

That question is more useful than memorizing architecture diagrams.

---

# 56. Interview Preparation

## Coding

Pick one primary interview language.

Recommended:

- Go
- Java
- C++

For your target identity, Go is useful. For broad enterprise hiring, Java remains highly valuable.

## Practice

- LeetCode
- NeetCode
- Codeforces occasionally
- AtCoder
- HackerRank for targeted basics

## Topics

```text
Arrays
Strings
Hash maps
Stacks
Queues
Linked lists
Trees
Graphs
Heaps
Binary search
Sliding window
Two pointers
Recursion
Backtracking
Dynamic programming
Greedy
Union-find
Topological sort
Shortest paths
```

Do not grind 500 random problems.

Target pattern recognition.

---

# 57. Engineering Interview Depth

Prepare:

- Linux
- networking
- OS
- databases
- concurrency
- distributed systems
- cloud
- Kubernetes
- debugging
- observability

You should be able to explain:

> What happens when I type google.com into a browser?

at multiple depths.

You should also be able to explain:

> What happens when a Kubernetes pod makes a request to another pod?

at multiple depths.

And:

> What happens when an LLM receives an inference request?

at multiple depths.

---

# 58. YouTube Channels / Creators

Use YouTube as a supplement, not the curriculum.

## Systems / backend

- Jon Gjengset
- Hussein Nasser
- Martin Kleppmann
- Gaurav Sen
- ByteByteGo
- ThePrimeagen
- Computerphile
- Ben Eater
- Low Level Learning
- Tsoding
- Jacob Sorber

## Linux / performance

- Brendan Gregg
- Linux Foundation
- Red Hat
- Cilium
- Liz Rice

## Cloud / Kubernetes

- CNCF
- KubeCon
- AWS
- Google Cloud
- Microsoft Azure
- TechWorld with Nana
- Bret Fisher

## AI systems

- NVIDIA
- NVIDIA Developer
- PyTorch
- Hugging Face
- Anyscale
- Databricks
- Google DeepMind
- Stanford
- Full Stack Deep Learning

## University courses

- MIT OpenCourseWare
- Stanford Online
- CMU
- Berkeley
- Harvard CS
- University of Washington

---

# 59. Video Series to Prioritize

## P0

### MIT 6.5840

Distributed systems lectures + labs.

### Stanford CS336

Language modeling from scratch.

### MIT xv6

Operating systems.

### CMU 15-213

Computer systems.

### NVIDIA GTC

AI infrastructure and accelerated computing.

### AWS re:Invent

Cloud architecture and production systems.

### KubeCon

Kubernetes and cloud-native systems.

---

# 60. Courses

## Systems

- MIT 6.S081
- CMU 15-213
- MIT 6.5840
- Berkeley CS162
- Stanford CS149
- Stanford distributed systems material

## AI

- Stanford CS336
- Stanford CS224N
- Stanford CS229
- Stanford CS329S
- Full Stack Deep Learning
- Hugging Face Course
- DeepLearning.AI
- NVIDIA DLI

## Cloud

- AWS Skill Builder
- AWS Ramp-Up Guides
- Kubernetes official tutorials
- Linux Foundation
- HashiCorp Learn

---

# 61. Full Stack Deep Learning

https://fullstackdeeplearning.com/

Use it for:

- ML system design
- deployment
- monitoring
- data
- model serving
- evaluation
- productionization

Do not spend too much time on generic model-building content. Your target is the systems layer.

---

# 62. MLSysBook

https://mlsysbook.ai/

Use as one of your central references.

Focus on:

- hardware
- systems
- model serving
- scaling
- distributed training
- inference
- performance
- cost

The project also provides:

- courses
- labs
- lecture slides
- experiments
- TinyTorch
- hardware-oriented learning resources

---

# 63. DeepLearning.AI

https://www.deeplearning.ai/

Use selectively.

Prioritize courses around:

- LLM serving
- inference
- quantization
- ML systems
- MLOps
- evaluation
- transformers
- performance

Ignore the temptation to take every new short course.

---

# 64. AI Infrastructure Tool Map

```text
MODEL
 |
PyTorch / JAX
 |
Transformers
 |
Compilation
 |
TorchInductor / Triton
 |
Serving
 |
vLLM / Triton / SGLang
 |
Orchestration
 |
Kubernetes
 |
KServe
 |
GPU
 |
CUDA / NCCL
 |
Infrastructure
 |
AWS / Terraform
 |
Observability
 |
OpenTelemetry / Prometheus / Grafana
```

Learn the relationships between these layers.

---

# 65. Cloud-Native Tool Map

```text
Linux
  |
Containers
  |
Docker / containerd / runc
  |
Kubernetes
  |
CNI / CSI / CRI
  |
Ingress / Gateway
  |
Service Mesh
  |
Observability
  |
OpenTelemetry
  |
Prometheus
  |
Grafana
  |
Infrastructure as Code
  |
Terraform
  |
AWS
```

---

# 66. Performance Tool Map

```text
Application
    |
pprof
    |
perf
    |
flamegraph
    |
eBPF
    |
bcc / bpftrace
    |
kernel tracing
    |
CPU / memory / I/O / network
```

For GPU:

```text
PyTorch Profiler
      |
Nsight Systems
      |
Nsight Compute
      |
CUDA kernel analysis
      |
GPU memory / occupancy / throughput
```

---

# 67. Developer Tools

Become extremely comfortable with:

- Git
- GitHub
- GitHub Actions
- Docker
- Kubernetes
- Terraform
- Make
- Bash
- tmux
- Vim/Neovim
- VS Code
- JetBrains
- curl
- jq
- yq
- ripgrep
- fd
- fzf

---

# 68. Git / GitHub

Learn:

- branching
- rebasing
- cherry-picking
- bisect
- reflog
- blame
- worktrees
- signed commits
- tags
- releases
- Actions
- issue templates
- PRs
- CODEOWNERS

Git documentation:

https://git-scm.com/doc

GitHub Skills:

https://skills.github.com/

---

# 69. Security for Infrastructure

You do not need to become a security specialist.

You do need security competence.

Learn:

- TLS
- IAM
- secrets
- key management
- OAuth
- JWT
- network security
- container isolation
- Kubernetes RBAC
- supply-chain security
- SBOMs
- image signing
- vulnerability scanning
- least privilege

Resources:

- OWASP: https://owasp.org/
- Kubernetes security docs
- AWS Security documentation
- NIST: https://www.nist.gov/
- CISA: https://www.cisa.gov/

---

# 70. Reliability Design Patterns

Learn deeply:

- retries
- exponential backoff
- jitter
- timeout
- circuit breaker
- bulkhead
- load shedding
- rate limiting
- admission control
- graceful degradation
- hedged requests
- caching
- replication
- failover
- health checks
- quorum
- idempotency

Important:

**Retries can make an outage worse.**

Understand retry storms, cascading failures and overload.

---

# 71. Distributed Failure Scenarios

Practice designing for:

```text
Single process crash
Node crash
Network partition
Packet loss
Packet duplication
Packet reordering
High latency
Clock skew
Disk failure
Database failure
Cache failure
Message broker failure
DNS failure
Certificate expiry
Credential expiry
Region failure
Zone failure
Bad deployment
Configuration corruption
Traffic spike
Dependency overload
```

For every project, intentionally create some of these failures.

---

# 72. Load Testing

Tools:

- k6
- wrk
- wrk2
- hey
- vegeta
- Locust
- JMeter
- Gatling

Learn:

- closed-loop vs open-loop load
- throughput
- latency distribution
- coordinated omission
- warmup
- steady state
- spike tests
- soak tests
- stress tests

Grafana k6 learning resources:

https://grafana.com/docs/k6/

---

# 73. Benchmarking

Every serious project should answer:

```text
How fast?
How much memory?
How much CPU?
How much network?
How does it scale?
What happens at saturation?
What happens under failure?
What is p99?
What is the cost?
```

Never publish:

> "My system is extremely fast."

Publish:

> "At 8 cores and 10 replicas, the system sustained X requests/sec at p99 Y ms under workload Z."

---

# 74. Technical Writing

Your blog should eventually resemble an engineering publication.

## Article types

### Build logs

"I Built X"

### Deep dives

"How X Works"

### Benchmark studies

"Benchmarking X vs Y"

### Failure analysis

"How X Failed"

### Architecture

"Designing X at Scale"

### Paper breakdowns

"Understanding X"

### Production lessons

"What I Learned Operating X"

### Experiments

"What Happens If We..."

---

# 75. Technical Writing Books

- The Elements of Style
- On Writing Well
- The Sense of Style
- Writing for Software Developers
- Docs for Developers
- The Staff Engineer's Path
- The Manager's Path
- The Pragmatic Programmer

---

# 76. Technical Blog References

Study how these engineers/organizations write:

- AWS Builders' Library
- Cloudflare
- Netflix
- Stripe
- Uber
- Slack
- Discord
- Google
- Meta
- GitHub
- Datadog
- Kubernetes
- Cilium
- Grafana
- OpenTelemetry
- NVIDIA
- Hugging Face
- vLLM
- KServe

Do not copy their voice.

Copy their discipline:

```text
Problem
Context
Constraints
Architecture
Tradeoffs
Implementation
Measurements
Failures
Lessons
```

---

# 77. Your Blog Architecture

Recommended sections:

```text
/
├── About
├── Projects
├── Articles
├── Research
├── Notes
├── Talks
├── Reading
└── Contact
```

## Categories

```text
AI Infrastructure
Distributed Systems
Cloud
Kubernetes
SRE
Systems Programming
Performance
Research
Engineering
```

---

# 78. Your Personal Brand

## Core statement

> I build the infrastructure that makes AI fast, reliable and scalable.

## Supporting themes

- AI infrastructure
- distributed systems
- cloud
- reliability
- systems programming
- performance

## Avoid

- generic motivational content
- fake productivity
- AI-generated "10 lessons" spam
- fake technical authority
- exaggerated project metrics
- pretending to have experience you do not have

---

# 79. Instagram

Use it as the human-facing layer of the brand.

## Content

- project demos
- conference photos
- architecture sketches
- terminal clips
- server-room photos
- engineering experiments
- short explainers
- career milestones
- occasional personal life

## Reels

Ideas:

```text
What happens when an EC2 instance dies?
Why Kubernetes kills your pod
Why LLMs need KV cache
How Raft elects a leader
I built a distributed rate limiter
Why p99 matters
What actually happens inside a container
Why GPUs are memory-bound
```

---

# 80. LinkedIn

Use it as professional distribution.

## Post formats

### Build post

```text
I built X.

The problem:
...

Architecture:
...

The interesting part:
...

Benchmark:
...

What surprised me:
...
```

### Learning post

```text
I spent the week understanding X.

The mental model that finally clicked:
...

```

### Incident post

Never reveal confidential information.

Write:

```text
Symptom
Impact
Detection
Root cause
Contributing factors
Fix
Prevention
Lesson
```

---

# 81. YouTube

Your YouTube should eventually contain deeper technical material.

## Video formats

### 5-10 minutes

Single concept.

### 15-30 minutes

Architecture breakdown.

### 30-60 minutes

Build from scratch.

### Series

"Building a Distributed Database"

"Building an LLM Inference Platform"

"Understanding Kubernetes Internals"

---

# 82. Speaking

Target:

### Year 1

- college meetup
- student club
- local developer meetup

### Year 2

- CNCF community event
- AWS community event
- regional conference

### Year 3

- major conference
- KubeCon
- AWS community / re:Invent ecosystem
- systems conference workshop

## First talk

Do not make it:

> "The Future of AI"

Make it:

> "I Built a Distributed Rate Limiter in Go: Architecture, Failure Modes and Benchmarks"

Specific beats grandiose.

---

# 83. Open Source + Public Learning Loop

Use this loop:

```text
Learn
  ↓
Build
  ↓
Benchmark
  ↓
Open source
  ↓
Write
  ↓
Publish
  ↓
Discuss
  ↓
Receive criticism
  ↓
Improve
  ↓
Contribute upstream
```

This is how your reputation compounds.

---

# 84. Community

Communities worth following:

- CNCF
- Kubernetes Slack
- Kubernetes GitHub
- Cilium Slack/community
- OpenTelemetry community
- Prometheus community
- Grafana community
- Rust community
- Go community
- AWS User Groups
- AWS Community Builders
- NVIDIA developer community
- Hugging Face community
- PyTorch community
- MLPerf community

Do not join 30 communities and lurk.

Participate in 2-3.

---

# 85. Conferences

## Cloud

- AWS re:Invent
- AWS Summits
- Google Cloud Next
- Microsoft Ignite

## Cloud Native

- KubeCon + CloudNativeCon
- Open Source Summit
- DockerCon

## Systems

- OSDI
- SOSP
- NSDI
- EuroSys
- FAST
- ATC

## AI

- GTC
- MLSys
- NeurIPS
- ICML
- ICLR
- PyTorch Conference

## Hardware

- Hot Chips
- SC Conference

---

# 86. Research Workflow

Use:

- Google Scholar
- Semantic Scholar
- arXiv
- Papers with Code
- DBLP
- OpenReview

## Paper note template

```markdown
# Paper: <Title>

## Problem

## Why Existing Systems Fail

## Key Idea

## Architecture

## Algorithm

## Tradeoffs

## Evaluation

## Limitations

## What I Learned

## How I Could Implement It

## Related Papers

## Potential Project
```

---

# 87. Paper Reading Workflow

### First pass

5-10 minutes.

Read:

- abstract
- intro
- figures
- conclusion

### Second pass

30-60 minutes.

Understand:

- architecture
- algorithm
- evaluation

### Third pass

Only if important.

Read:

- implementation details
- proofs
- limitations
- appendix

Do not spend 4 hours understanding a paper that has no relevance to your current work.

---

# 88. Engineering Notebook

Maintain one repository:

```text
engineering-notes/
├── distributed-systems/
├── linux/
├── networking/
├── kubernetes/
├── aws/
├── gpu/
├── llm-inference/
├── reliability/
├── performance/
├── papers/
└── experiments/
```

Each note should contain:

```text
Concept
Mental model
Diagram
Example
Failure mode
Experiment
Questions
References
```

---

# 89. Your Obsidian Vault

Suggested structure:

```text
00 Inbox
01 Foundations
02 Linux
03 Networking
04 Systems
05 Distributed Systems
06 Databases
07 Cloud
08 Kubernetes
09 SRE
10 Observability
11 GPU
12 AI Infrastructure
13 Papers
14 Projects
15 Blog
16 Talks
17 Career
18 Content
19 Books
20 People
```

Use backlinks aggressively.

---

# 90. Knowledge Graph

Connect concepts:

```text
Raft
 ↓
Replication
 ↓
Fault tolerance
 ↓
Distributed KV
 ↓
etcd
 ↓
Kubernetes
 ↓
Control plane
```

Another:

```text
KV cache
 ↓
GPU memory
 ↓
PagedAttention
 ↓
vLLM
 ↓
Inference throughput
 ↓
GPU cost
 ↓
Autoscaling
```

Another:

```text
SLO
 ↓
Latency
 ↓
Tracing
 ↓
OpenTelemetry
 ↓
Prometheus/Grafana
 ↓
Incident response
```

This is how isolated knowledge becomes systems thinking.

---

# 91. Reading List: Core Books

## Systems

- Computer Systems: A Programmer's Perspective
- Operating Systems: Three Easy Pieces
- The Linux Programming Interface
- Linux Kernel Development
- Systems Performance
- The Art of Computer Programming, selective reference

## Networking

- Computer Networking: A Top-Down Approach
- TCP/IP Illustrated
- Unix Network Programming
- High Performance Browser Networking

## Distributed Systems

- Designing Data-Intensive Applications
- Database Internals
- Distributed Systems — van Steen & Tanenbaum
- Understanding Distributed Systems
- Designing Distributed Systems
- Distributed Algorithms

## Databases

- Database System Concepts
- Database Internals
- Readings in Database Systems
- Transaction Processing

## SRE

- Site Reliability Engineering
- The Site Reliability Workbook
- Seeking SRE
- Database Reliability Engineering

## Cloud

- Cloud Architecture Patterns
- AWS Well-Architected materials
- AWS Builders' Library

## AI/ML Systems

- Designing Machine Learning Systems
- Machine Learning Engineering
- Machine Learning Systems
- Build a Large Language Model From Scratch
- Natural Language Processing with Transformers

## Programming

- The Go Programming Language
- Learning Go
- 100 Go Mistakes
- Programming Rust
- Rust for Rustaceans
- Zero To Production in Rust
- The C Programming Language
- Effective C

## Engineering / Career

- The Pragmatic Programmer
- Staff Engineer — Will Larson
- The Staff Engineer's Path — Tanya Reilly
- The Manager's Path — Camille Fournier
- Accelerate
- Release It!

---

# 92. Books You Should NOT Read Yet

Avoid premature complexity.

Do not prioritize:

- advanced compiler theory
- advanced cryptography
- advanced formal verification
- obscure distributed algorithms
- massive ML theory textbooks
- GPU compiler internals
- advanced category theory

unless your project requires them.

Your bottleneck is not lack of resources.

Your bottleneck will be execution and depth.

---

# 93. Resource Selection Rule

When choosing between 20 resources:

Choose:

```text
1 textbook
1 course
1 official documentation set
1 practical project
1 paper
1 source-code base
```

Then build.

Example:

```text
Distributed systems

Book:
DDIA

Course:
MIT 6.5840

Docs:
etcd / Kubernetes

Project:
Distributed KV

Paper:
Raft

Source:
etcd
```

That is enough to become substantially better.

---

# 94. Source Code Reading

Do not only read source code after becoming "advanced."

Start small.

## Read

### Go

- net/http
- context
- sync
- runtime

### Rust

- tokio
- axum
- hyper

### Distributed

- etcd
- TiKV
- CockroachDB
- Redis
- RocksDB

### Kubernetes

- scheduler
- kubelet
- controller-runtime

### AI

- vLLM
- Triton
- KServe
- Ray
- PyTorch

---

# 95. Source Code Reading Method

Never open a 500,000-line repository and randomly browse.

Start:

```text
README
 ↓
Architecture docs
 ↓
Entry point
 ↓
One request path
 ↓
One data path
 ↓
One failure path
 ↓
Tests
 ↓
Implementation
```

Then trace:

```text
request
→ API
→ router
→ scheduler
→ worker
→ storage
→ response
```

---

# 96. Your Engineering Metrics

Track monthly:

## Engineering

- projects completed
- benchmarks completed
- systems deployed
- incidents simulated
- source code read

## Knowledge

- concepts mastered
- books completed
- papers understood
- courses completed

## Open source

- issues opened
- PRs submitted
- PRs merged
- upstream contributions

## Public work

- articles
- videos
- talks
- meaningful discussions

## Career

- ownership
- production systems
- design documents
- incidents handled
- performance improvements

## Audience

- relevant followers
- email subscribers
- article views
- technical discussions
- inbound opportunities

---

# 97. The 90-Day Sprint

The reference plan gives the first 90 days a concrete focus.

## Engineering

- Choose Distributed Rate Limiter in Go.
- Implement core functionality.
- Add Redis.
- Add Docker.
- Add integration tests.
- Add distributed coordination.
- Benchmark throughput and latency.
- Document architecture.

## Knowledge

- Study Raft.
- Study consistent hashing.
- Strengthen AWS networking and IAM.
- Study Kubernetes networking and scheduling.
- Learn OpenTelemetry and distributed tracing.

## Brand

- Update Instagram.
- Update LinkedIn.
- Publish 12 substantial LinkedIn posts.
- Publish 6 Instagram reels.
- Write 2 technical articles.
- Record one project demo.

## Community

- Identify 10 AI infrastructure engineers.
- Contribute meaningfully to one open-source project.
- Attend one technical meetup.

---

# 98. 12-Month Roadmap

## Months 1-3

### Focus

Systems + Go + AWS + distributed systems.

### Build

Distributed Rate Limiter.

### Study

- CS:APP
- DDIA
- MIT 6.5840
- AWS networking
- Kubernetes fundamentals

### Publish

- 2 technical articles
- 12 LinkedIn posts
- 6 short videos/reels

---

# 99. Months 4-6

### Focus

Storage + Kubernetes + observability.

### Build

Distributed KV Store.

### Learn

- Raft
- WAL
- SSTables
- compaction
- Kubernetes internals
- OpenTelemetry
- Prometheus
- Grafana

### Publish

- KV architecture article
- Raft implementation article
- performance benchmark

---

# 100. Months 7-9

### Focus

GPU + LLM inference.

### Learn

- CUDA
- PyTorch internals
- GPU memory
- vLLM
- Triton
- batching
- KV cache
- quantization

### Courses

- Stanford CS336
- NVIDIA DLI
- Hugging Face Course

### Build

Mini inference server.

---

# 101. Months 10-12

### Focus

Production AI infrastructure.

### Build

LLM inference platform.

### Stack

```text
AWS
Kubernetes
vLLM
GPU
Terraform
Prometheus
Grafana
OpenTelemetry
```

### Publish

- architecture
- benchmarks
- cost analysis
- production lessons

### Speak

Give your first technical talk.

---

# 102. Year 2

## Primary goal

Become genuinely strong at AI systems.

### Learn

- GPU architecture
- distributed inference
- distributed training
- NCCL
- CUDA
- model parallelism
- tensor parallelism
- pipeline parallelism
- quantization
- speculative decoding
- scheduling

### Build

A serious inference platform.

### Contribute

To vLLM, Kubernetes, KServe, Triton, Ray, PyTorch, Cilium or OpenTelemetry.

### Publish

One substantial article every 2-4 weeks.

---

# 103. Year 3

## Primary goal

Authority.

### Produce

- research-style engineering reports
- serious open-source contributions
- conference talks
- technical videos
- production architecture articles

### Career

Target:

- AWS
- NVIDIA
- Google
- Microsoft
- Meta
- Databricks
- Cloudflare
- Anthropic
- OpenAI
- leading infrastructure startups
- strong distributed systems organizations

The employer matters less than the engineering environment.

---

# 104. Your Weekly Learning System

## Monday

Read one concept deeply.

## Tuesday

Implement it.

## Wednesday

Debug and test it.

## Thursday

Benchmark it.

## Friday

Write about it.

## Saturday

Open source / project work.

## Sunday

Review + plan + rest.

---

# 105. Weekly Time Budget

Reference target:

| Activity | Hours |
|---|---:|
| Deep technical learning | 8 |
| Projects | 8 |
| Content | 4 |
| Papers / engineering blogs | 3 |
| Networking / open source | 2 |
| **Total** | **25** |

This is ambitious. Your job/studies come first. Consistency matters more than forcing 25 hours every week.

---

# 106. Daily Deep Work Rule

Protect at least one uninterrupted technical block.

During it:

- phone away
- no Instagram
- no LinkedIn
- no random YouTube
- no AI chatbot hopping
- no tutorial switching

One problem.

One system.

One objective.

---

# 107. Anti-Resource-Hoarding Rule

You are especially vulnerable to collecting technologies.

Do not create a new learning list every week.

Use:

```text
Current skill
↓
Current project
↓
Required concept
↓
One resource
↓
Implementation
```

Example:

```text
Project:
Rate limiter

Need:
Consistent hashing

Read:
DDIA + one implementation reference

Build:
Consistent hash ring

Test:
Node addition/removal

Publish:
Engineering writeup
```

Then move on.

---

# 108. How to Turn Learning into Public Content

One project can produce 10+ pieces.

Example:

## Distributed Rate Limiter

1. Why rate limiting exists
2. Token bucket
3. Sliding window
4. Distributed rate limiting
5. Consistent hashing
6. Redis design
7. Failure modes
8. Benchmarking
9. Kubernetes deployment
10. Architecture video
11. Full technical article
12. Conference talk

This is how you avoid constantly inventing content.

---

# 109. Your Content Flywheel

```text
Project
  ↓
Experiment
  ↓
Benchmark
  ↓
Article
  ↓
LinkedIn post
  ↓
Instagram reel
  ↓
YouTube video
  ↓
Talk
  ↓
Open-source repository
  ↓
Community discussion
  ↓
New project
```

One engineering project becomes an entire content ecosystem.

---

# 110. Personal Brand Rule

Do not optimize for:

```text
likes
followers
viral posts
aesthetic screenshots
```

Optimize for:

```text
technical credibility
usefulness
consistency
recognition
relationships
proof of work
```

Audience growth should be a side effect of being useful.

---

# 111. What "Expert" Should Mean

You are not an expert because:

- you watched a Kubernetes course
- you have an AWS certificate
- you know 30 tools
- you have 100K followers
- you can explain CAP theorem

You become expert-like when you can:

- design
- implement
- benchmark
- operate
- debug
- explain
- improve
- critique

real systems.

---

# 112. The Skill Test

For every technology, ask:

### Level 1

Can I use it?

### Level 2

Can I explain it?

### Level 3

Can I debug it?

### Level 4

Can I design with it?

### Level 5

Can I operate it?

### Level 6

Can I modify its internals?

### Level 7

Can I explain its tradeoffs to another engineer?

Aim for Level 5-7 on your core technologies.

You do not need Level 7 on everything.

---

# 113. Your Core Technology Set

## Tier 1: Deep

```text
Linux
Go
Distributed Systems
AWS
Kubernetes
Networking
Observability
LLM Inference
GPU fundamentals
```

## Tier 2: Strong

```text
Rust
Python
Docker
Terraform
PostgreSQL
Redis
Kafka
Prometheus
Grafana
OpenTelemetry
CUDA
```

## Tier 3: Familiar

```text
C++
Java
Ray
KServe
Triton
SageMaker
Flink
Spark
NATS
Pulsar
```

This prevents tool-collection addiction.

---

# 114. Research Mindset

When you encounter a new technology, ask:

```text
What problem does this solve?
Why does the problem exist?
What existed before it?
What tradeoff does it make?
What assumptions does it make?
Where does it fail?
How does it scale?
How does it recover?
How is it observed?
What does it cost?
```

That questioning style is more valuable than memorizing documentation.

---

# 115. Your Long-Term Technical Thesis

A useful thesis for your career:

> **AI is becoming a systems problem.**

The interesting work is increasingly at the intersection of:

```text
AI
+
Distributed Systems
+
Cloud
+
Hardware
+
Networking
+
Operating Systems
+
Reliability
```

That intersection is where you should build depth.

---

# 116. Final Resource Priority

If you ignored everything else and only followed this list, I would choose:

## Tier S

1. CS:APP
2. OSTEP
3. MIT 6.5840
4. DDIA
5. AWS Builders' Library
6. AWS Ramp-Up Guides
7. Kubernetes official docs
8. Linux documentation
9. Site Reliability Engineering book
10. Stanford CS336
11. MLSysBook
12. NVIDIA CUDA resources
13. vLLM docs
14. NVIDIA Triton docs
15. OpenTelemetry docs

## Tier A

16. Database Internals
17. Systems Performance
18. Rust Book
19. Go Tour + Effective Go
20. KServe docs
21. Ray docs
22. Prometheus docs
23. Grafana Learn
24. Hugging Face Course
25. Full Stack Deep Learning
26. CS162
27. MIT xv6
28. CMU 15-213
29. NVIDIA DLI
30. Chip Huyen's ML systems work

## Tier B

31. Kafka Definitive Guide
32. Designing Distributed Systems
33. Understanding Distributed Systems
34. Rust for Rustaceans
35. Programming Rust
36. Linux Kernel Development
37. BPF Performance Tools
38. Network Programming with Go
39. System Design Interview
40. Machine Learning Engineering

---

# 117. The 15 Resources I Would Start Tomorrow

If the entire library feels overwhelming, ignore 95% of it.

Start with:

1. **MIT 6.5840**
2. **Designing Data-Intensive Applications**
3. **CS:APP**
4. **OSTEP**
5. **AWS Ramp-Up Guide: Solutions Architect**
6. **AWS Builders' Library**
7. **Kubernetes official docs**
8. **Go Tour**
9. **Linux documentation**
10. **Site Reliability Engineering**
11. **Stanford CS336**
12. **NVIDIA CUDA learning**
13. **vLLM documentation**
14. **OpenTelemetry documentation**
15. **MLSysBook**

And build the distributed rate limiter alongside them.

---

# 118. Your Definition of Done at 24

You do **not** need to have mastered every resource in this document.

You should ideally be able to say:

```text
I have operated production infrastructure.

I understand Linux deeply.

I can design distributed systems.

I can build them in Go/Rust.

I understand AWS architecture.

I can operate Kubernetes.

I understand observability and SRE.

I understand GPU and LLM inference infrastructure.

I have built serious systems.

I have contributed to open source.

I have published substantial technical writing.

I have given technical talks.

I have a public engineering portfolio.

People know me for a specific technical niche.

My reputation comes from actual engineering work.
```

That is the real target.

---

# 119. The One-Sentence Strategy

> **Build difficult systems, understand them deeply, measure them honestly, publish what you learn, contribute upstream, and repeat for three years.**

That is the shortest version of the entire roadmap.

---

# 120. Reference Links: Core Hubs

## Cloud

- AWS: https://aws.amazon.com/
- AWS Training: https://aws.amazon.com/training/
- AWS Ramp-Up Guides: https://aws.amazon.com/training/ramp-up-guides/
- AWS Architecture: https://aws.amazon.com/architecture/
- AWS Builders' Library: https://aws.amazon.com/builders-library/
- AWS Well-Architected: https://aws.amazon.com/architecture/well-architected/

## Systems

- Linux: https://kernel.org/
- Linux docs: https://docs.kernel.org/
- man7: https://man7.org/
- CMU CSAPP: https://csapp.cs.cmu.edu/
- MIT PDOS: https://pdos.csail.mit.edu/
- OSTEP: https://pages.cs.wisc.edu/~remzi/OSTEP/

## Distributed systems

- MIT 6.5840: https://pdos.csail.mit.edu/6.5840/
- Raft: https://raft.github.io/
- DDIA: https://dataintensive.net/
- Distributed Systems: https://www.distributed-systems.net/

## Cloud native

- Kubernetes: https://kubernetes.io/
- CNCF: https://www.cncf.io/
- Docker: https://www.docker.com/
- Terraform: https://developer.hashicorp.com/terraform
- Cilium: https://cilium.io/

## Observability

- OpenTelemetry: https://opentelemetry.io/
- Prometheus: https://prometheus.io/
- Grafana: https://grafana.com/
- Jaeger: https://www.jaegertracing.io/

## AI

- PyTorch: https://pytorch.org/
- Hugging Face: https://huggingface.co/
- NVIDIA: https://developer.nvidia.com/
- CUDA: https://developer.nvidia.com/cuda-zone
- vLLM: https://docs.vllm.ai/
- Triton: https://docs.nvidia.com/deeplearning/triton-inference-server/
- KServe: https://kserve.github.io/
- Ray: https://docs.ray.io/
- MLSysBook: https://mlsysbook.ai/
- Full Stack Deep Learning: https://fullstackdeeplearning.com/

## Research

- arXiv: https://arxiv.org/
- Semantic Scholar: https://www.semanticscholar.org/
- DBLP: https://dblp.org/
- Google Research: https://research.google/
- Amazon Science: https://www.amazon.science/
- Microsoft Research: https://www.microsoft.com/en-us/research/
- NVIDIA Research: https://research.nvidia.com/
- OpenReview: https://openreview.net/

---

# 121. Final Operating Principle

The goal is not to finish this document.

The goal is to make the document increasingly unnecessary.

Every month, fewer things should remain theoretical.

More should be:

```text
implemented
deployed
measured
broken
debugged
optimized
documented
published
```

By 24, the strongest version of your portfolio is not the one with the most impressive design.

It is the one where every claim on the website can be backed by **real systems, real measurements, real production experience, real research, and real engineering judgment.**
