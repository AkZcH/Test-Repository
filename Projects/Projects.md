- distributed inference scheduler
- GPU workload orchestrator
- high-throughput feature/embedding service
- production-grade RAG infrastructure
- distributed configuration/control plane
- Kubernetes operator
- observability platform
- [[AI Inference Platform Project]]

- Distributed Rate Limiter
- KV Store
- Load Balancer
- URL ShortenerAI inference/serving system**
vLLM/Triton + Kubernetes + benchmarking + observability.
-  Distributed AI platform
Queues, workers, autoscaling, failure recovery, caching, scheduling, tracing, cost controls.
- AI data/RAG infrastructure
Ingestion → chunking → embeddings → retrieval → reranking → evaluation → observability.


---

Akshat, I'd structure your roadmap around one goal: become an engineer who can build, explain, debug, and scale infrastructure systems from first principles.

Given your SRE background at HighRadius and your interests in Go, Rust, distributed systems, and AI infrastructure, don't build 20 unrelated CRUD applications. Build a sequence where each project teaches you something that the next project depends on.

The order below combines projects we've discussed previously with the systems I'd add to make the progression complete.

# Infrastructure Engineering Project Roadmap

30 projects · ordered by dependency and learning value · Go, Rust, Python, TypeScript

## Phase 1: Core systems fundamentals

Goal: understand memory, concurrency, algorithms, networking, and performance.

## Your master checklist

### 0/29

### Phase 1 · Fundamentals

LRU/LFU Cache

Rust

Hash maps, linked lists, eviction policies, benchmarks.

Thread Pool / Worker Pool

Go

Goroutines, bounded queues, cancellation, backpressure.

In-Memory Key-Value Store

Go

GET/SET/DELETE, TTL, persistence, concurrent access.

TCP Server and Protocol Parser

Rust

Sockets, framing, partial reads, timeouts, connection handling.

### Phase 2 · Essential backend infrastructure

Go Rate Limiter

Go

Token bucket, sliding window, concurrency, Redis-backed distributed limits.

URL Shortener

Go + PostgreSQL + Redis

ID generation, redirects, caching, analytics, rate limiting.

Reverse Proxy and Load Balancer

Go

HTTP proxying, round robin, least connections, health checks.

Metrics and Observability Agent

Go

Metrics collection, Prometheus exposition, latency histograms, alerting basics.

Log Aggregation Service

Go + OpenTelemetry

Structured logs, ingestion, indexing, querying, retention.

### Phase 3 · Messaging and asynchronous systems

MQ: Your Own RabbitMQ-Inspired Broker

Go

Producers, consumers, acknowledgements, retries, dead-letter queues, persistence.

Pub/Sub Event Broker

Go or Rust

Topics, partitions, consumer groups, offsets, replay.

Distributed Job Scheduler

Go

Scheduling, worker leases, retries, delayed jobs, failure recovery.

Workflow Orchestrator

Go

DAG execution, dependencies, durable state, retries, recovery.

### Phase 4 · Distributed systems

Distributed KV Store

Go

Replication, partitioning, consistent hashing, failure handling.

Raft-Based Replicated Store

Go

Leader election, log replication, quorum, snapshots.

Distributed Cache

Go

Redis-like API, TTL, eviction, replication, cache invalidation.

Service Discovery and Configuration

Go

Registration, health checks, leases, dynamic configuration.

API Gateway

Go

Routing, authentication, rate limiting, timeouts, circuit breakers.

Object Storage Service

Go

Multipart uploads, metadata, checksums, replication, durability.

Fault Injection and Chaos Testing Tool

Go

Latency, packet loss, process failures, recovery measurement.

### Phase 5 · AI infrastructure

LLM Inference Server

Python + PyTorch

Model loading, token generation, streaming, batching, latency measurement.

LLM KV Cache Manager

Python + CUDA fundamentals

Attention KV tensors, memory allocation, block management, cache reuse.

Inference Router and Load Balancer

Go + Python

Model routing, queue-aware scheduling, health checks, load shedding.

Continuous Batching Engine

Python + PyTorch

Dynamic batching, request scheduling, throughput versus latency.

Vector Database

Rust or Go

Vector indexing, similarity search, metadata filtering, persistence.

Distributed Model Serving Platform

Go + Python

Model deployment, autoscaling, GPU metrics, rollout, observability.

### Phase 6 · Advanced systems

Mini Container Runtime

Go or Rust

Namespaces, cgroups, process isolation, filesystem setup, Linux internals.

eBPF Observability Tool

Go + C/eBPF

Kernel events, syscall/network tracing, latency and resource attribution.

End-to-End Infrastructure Platform

Go + PostgreSQL + Redis + OpenTelemetry

Integrate the gateway, rate limiter, MQ, workers, cache, metrics and fault testing.

Copy checklist

## Recommended technology stack

|Language / tool|Where to use it|
|---|---|
|Go|Rate limiter, MQ, load balancer, API gateway, schedulers, distributed services|
|Rust|KV store, protocol implementations, performance-critical systems, eBPF tooling|
|Python + PyTorch|LLM inference, batching, model serving, AI experiments|
|TypeScript|Optional dashboard for visualizing metrics, queues, and system health|
|PostgreSQL|Durable relational metadata and application state|
|Redis|Cache, rate-limit counters, ephemeral state|
|Docker + Linux|Packaging, isolation, deployment, debugging|
|Prometheus + Grafana|Metrics, dashboards, alerting|
|OpenTelemetry|Traces, metrics, instrumentation|
|gRPC + Protobuf|Typed communication between distributed services|
|Kubernetes|Deployment and orchestration when the project genuinely needs it|
|AWS|S3, EC2, networking, IAM, and deployment infrastructure|

## The rules I would follow

1. Build one project at a time. Finish a working MVP before starting the next.
    
2. Benchmark before optimizing. Measure throughput, p50/p95/p99 latency, memory, and CPU.
    
3. Inject failures. Test crashes, network delays, duplicate messages, and restarts.
    
4. Write design documents. Explain architecture, trade-offs, limitations, and failure modes.
    
5. Avoid premature distribution. Make a correct single-node version before adding replication and sharding.
    

My recommendation: Start with LRU cache, then your Go rate limiter, URL shortener, reverse proxy/load balancer, and your own MQ. After that, tackle distributed storage and Raft before moving into LLM serving and KV-cache management.

One important distinction: an ordinary key-value store and an LLM inference KV cache are different projects. Build the first to learn storage systems; build the second to understand AI inference infrastructure.

You do not need 30 finished repositories to become a strong infrastructure engineer. Five deeply engineered projects with benchmarks, failure testing, and clear design documentation will teach you more than 30 superficial implementations.