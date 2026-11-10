
**Window:** October 2026 to April 2027 (26 weeks) **Goal:** Become the SRE who breaks AI systems on purpose, so they don't break in production. **Focus areas:** LLM inference and serving, reliability and observability, distributed systems and systems programming, GPU performance.

---
Research Papers: [[AI Infrastructure Research Papers]]
FAANG Guide: [[]]

---

## 1. Where to start

Start with **inference serving plus observability**, not GPUs and not theory.

Why:

1. It is where your public evidence is thinnest and where the brand needs proof fastest.
2. It connects directly to your SRE internship, so work and learning reinforce each other.
3. It produces the first flagship project, which feeds weeks of content.
4. You already have a head start in the other two areas: your Rust write-ahead log engine (distributed systems) and your CUDA softmax kernel (GPU). Those run in the background and get deepened later, not started from zero.

**Order of attack:**

|Priority|Track|Why this position|
|---|---|---|
|1|LLM inference and serving|Where most failure stories live|
|2|Reliability and observability|Your SRE edge, pairs with track 1|
|3|Distributed systems and systems programming|Runs in parallel via your existing Rust work|
|4|GPU performance|Deepens after you have seen real serving bottlenecks|

---

## 2. Weekly rhythm (about 10 to 12 hours per week)

You are balancing an internship and final year, so the plan assumes a realistic load.

|When|Time|What|
|---|---|---|
|Mon, Wed, Thu evenings|1.5 h each|Reading or course material for the current phase|
|Tue or Fri evening|1.5 h|Rust or systems practice (the parallel track)|
|Saturday|3 to 4 h|Hands-on build or experiment on the GPU box|
|Sunday|1 to 2 h|Write the post, update notes, plan next week|

If a week gets crushed by exams or work, skip reading first, then skip the parallel track. Never skip the Saturday build.

---

## 3. Phase overview

|Phase|Weeks|Theme|Output|
|---|---|---|---|
|0|1 to 2|Setup and baseline|Working GPU environment, repo skeleton|
|1|3 to 6|Inference fundamentals and first deployment|vLLM served and benchmarked|
|2|7 to 10|Observability and SLOs for inference|Dashboards, alerts, SLOs|
|3|11 to 14|Load, tail latency, failure injection|**Flagship project 1 complete**|
|4|15 to 19|GPU performance|Profiled kernels, GPU failure write-ups|
|5|20 to 24|Distributed systems for AI workloads|Queue-based inference platform, **flagship project 2 started**|
|6|25 to 26|Consolidate and publish|Article, portfolio, 6-month review|

**Parallel track (all phases):** Rust and distributed systems, about 1.5 hours per week. Details in section 10.

---

## Phase 0: Setup and baseline (weeks 1 to 2)

**Goal:** A working environment and a habit, before any deep learning.

**Do:**

- [ ] Create a monorepo or an organized GitHub org for the flagship project (for example `ai-infra-lab`).
- [ ] Get GPU access. Cheap order of preference: Google Colab or Kaggle for first experiments, then RunPod, Lambda Labs, or Modal for hourly rentals. Set a spending cap before you start.
- [ ] Install Docker, kubectl, and a local Kubernetes option (kind or minikube) on your Linux machine.
- [ ] Run a small model locally with llama.cpp or Ollama just to see tokens come out.
- [ ] Write a first-principles note: "What happens during one LLM request?" in your own words, even if wrong. You will revise it later.
- [ ] Start a `notes/` folder. Every experiment gets a dated markdown file with setup, numbers, and surprises. These notes become posts.

**Resources:**

- Andrej Karpathy, "Intro to Large Language Models" (YouTube, about 1 hour) for the mental model.
- Andrej Karpathy, "Let's build GPT from scratch" (YouTube) for how a transformer actually generates tokens.
- llama.cpp repo: https://github.com/ggml-org/llama.cpp
- Docker docs: https://docs.docker.com
- kind: https://kind.sigs.k8s.io

**Post idea:** "I'm starting a 6-month public roadmap to learn AI infrastructure from the SRE side. Here's the plan."

---

## Phase 1: Inference fundamentals and first deployment (weeks 3 to 6)

**Goal:** Understand why LLM inference is expensive, and serve a model with vLLM.

### Week 3: How inference works

**Learn:** prefill vs decode, memory-bound vs compute-bound, why the KV cache exists and how big it gets.

**Do:** On paper, compute the KV cache size for a 7B model at different sequence lengths and batch sizes. Check your numbers against what vLLM reports at startup.

**Resources:**

- kipply, "Transformer Inference Arithmetic": https://kipp.ly/transformer-inference-arithmetic/
- Lilian Weng, "Large Transformer Model Inference Optimization": https://lilianweng.github.io/posts/2023-01-10-inference-optimization/
- Paper: "Attention Is All You Need" (arXiv 1706.03762), skim for architecture only.

### Week 4: Serving engines and batching

**Learn:** static vs continuous batching, PagedAttention, scheduler behavior, max batch size and max sequence length trade-offs.

**Do:** Deploy a small open model (a 1B to 8B Llama or Qwen variant) with vLLM in Docker on a rented GPU. Hit it with the OpenAI-compatible API.

**Resources:**

- vLLM docs: https://docs.vllm.ai
- Paper: "Efficient Memory Management for Large Language Model Serving with PagedAttention" (arXiv 2309.06180)
- Paper: "Orca: A Distributed Serving System for Transformer-Based Generative Models" (OSDI 2022), the origin of continuous batching.
- vLLM blog posts on PagedAttention and the V1 architecture (find them on the vLLM docs and blog).

### Week 5: Benchmarking properly

**Learn:** time to first token, inter-token latency, throughput in tokens per second, p50 vs p95 vs p99, open-loop vs closed-loop load.

**Do:** Benchmark your vLLM server with the benchmark scripts in the vLLM repo and with GuideLLM at concurrency 1, 8, 32, 64. Record everything in a table.

**Resources:**

- vLLM repo benchmark directory: https://github.com/vllm-project/vllm
- GuideLLM: https://github.com/vllm-project/guidellm
- Paper: "The Tail at Scale" (Dean and Barroso, CACM 2013), required reading for the whole roadmap.

### Week 6: Compare engines

**Do:** Run the same model and same load on a second engine (SGLang first, then optionally TensorRT-LLM or llama.cpp). Compare latency and throughput honestly, including what you misconfigured.

**Resources:**

- SGLang: https://github.com/sgl-project/sglang
- NVIDIA TensorRT-LLM docs: https://nvidia.github.io/TensorRT-LLM/
- Triton Inference Server docs: https://docs.nvidia.com/deeplearning/triton-inference-server/

**Posts:**

1. "How big is the KV cache, really? I calculated it, then measured it."
2. "vLLM at concurrency 1, 8, 32, 64: where throughput stops scaling."
3. "I ran vLLM and SGLang on the same load. Here's what changed and what didn't."

---

## Phase 2: Observability and SLOs for inference (weeks 7 to 10)

**Goal:** Make the serving stack observable the way you would any production service, then define SLOs. This is your SRE edge.

### Week 7: Metrics pipeline

**Do:** Scrape vLLM's `/metrics` endpoint with Prometheus. Build a Grafana dashboard for request rate, queue depth, running vs waiting requests, KV cache utilization, time to first token, and inter-token latency.

**Resources:**

- Prometheus docs: https://prometheus.io/docs/
- Grafana docs: https://grafana.com/docs/
- vLLM metrics documentation (in the vLLM docs, search "metrics" and "production metrics").
- Book: "Observability Engineering" (Majors, Fong-Jones, Miranda).

### Week 8: GPU metrics

**Do:** Deploy NVIDIA DCGM Exporter and add GPU utilization, memory used, memory bandwidth, power, and temperature panels next to the serving panels. Learn which GPU metrics lie (utilization percentage is not efficiency).

**Resources:**

- DCGM Exporter: https://github.com/NVIDIA/dcgm-exporter
- NVIDIA DCGM docs: https://docs.nvidia.com/datacenter/dcgm/latest/

### Week 9: Tracing and logs

**Do:** Instrument a thin gateway in front of vLLM with OpenTelemetry. Trace one request through gateway, queue, prefill, and decode. Send traces to Tempo or Jaeger and logs to Loki.

**Resources:**

- OpenTelemetry docs: https://opentelemetry.io/docs/
- Grafana Tempo: https://grafana.com/docs/tempo/latest/
- Grafana Loki: https://grafana.com/docs/loki/latest/
- Langfuse (LLM-level tracing): https://langfuse.com/docs

### Week 10: SLOs and alerting

**Do:** Define 3 to 4 SLIs and SLOs for your inference service (for example, 99% of requests get first token under 1 second; availability 99.5%). Implement burn-rate alerts. Use Sloth or Pyrra to generate recording rules.

**Resources:**

- Google SRE Book (free): https://sre.google/sre-book/table-of-contents/
- Google SRE Workbook, chapter "Implementing SLOs" (free): https://sre.google/workbook/table-of-contents/
- Sloth: https://github.com/slok/sloth
- Pyrra: https://github.com/pyrra-dev/pyrra
- Book: "Implementing Service Level Objectives" (Alex Hidalgo).

**Posts:**

1. "The 6 metrics I'd put on every LLM inference dashboard."
2. "GPU utilization says 95%. Why that number is misleading."
3. "What SLOs should an inference service have? My first draft."

---

## Phase 3: Load, tail latency, and failure injection (weeks 11 to 14)

**Goal:** Break the service on purpose, detect each failure with your dashboards, and finish flagship project 1. This phase produces the posts that make the "break AI systems on purpose" line real.

### Week 11: Find the limits

**Do:** Run open-loop load tests that ramp request rate until latency collapses. Find the knee of the curve. Vary prompt length, output length, and `max-num-seqs` and `gpu-memory-utilization`.

**Resources:**

- k6 docs: https://grafana.com/docs/k6/latest/
- oha: https://github.com/hatoo/oha
- Locust: https://docs.locust.io

### Week 12: Make it fail (resource exhaustion)

**Do:** Trigger and document each of these, with the metric that warned you first:

- [ ] KV cache exhaustion and request preemption
- [ ] GPU out-of-memory from oversized context or batch
- [ ] Queue buildup under a burst
- [ ] A single long request hurting everyone else (head-of-line blocking)

### Week 13: Make it fail (infrastructure faults)

**Do:** Deploy on Kubernetes with Helm, requests, and limits. Inject faults: kill the pod mid-stream, add network latency and loss with `tc netem` or Toxiproxy, throttle the node. Observe client behavior and recovery time.

**Resources:**

- Kubernetes docs: https://kubernetes.io/docs/home/
- NVIDIA GPU Operator: https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/
- KServe: https://kserve.github.io/website/
- KEDA: https://keda.sh
- Chaos Mesh: https://chaos-mesh.org
- Toxiproxy: https://github.com/Shopify/toxiproxy
- Book: "Chaos Engineering" (Rosenthal and Jones).

### Week 14: Package flagship project 1

**Do:** Write the README, architecture diagram (Mermaid or Excalidraw), benchmark results, runbook, and a short postmortem for each failure. Record a 3 to 5 minute demo.

**Deliverables checklist:**

- [ ] GitHub repo with reproducible setup (one command to deploy)
- [ ] Architecture diagram
- [ ] Benchmark tables and charts
- [ ] Dashboards exported as JSON
- [ ] SLO definitions and alert rules
- [ ] Postmortems for at least 4 failures
- [ ] Demo video
- [ ] Technical article (long form)

**Posts (this is your _Breaking AI Infra_ series):**

1. "I sent 64 concurrent requests to a vLLM server. Here's exactly where it fell over."
2. "What happens to an inference service when the KV cache fills up."
3. "Why your p50 looks fine and your p99 is on fire."
4. "I killed the pod mid-generation. Here's what the client saw."
5. Final: the full write-up and repo release.

---

## Phase 4: GPU performance (weeks 15 to 19)

**Goal:** Explain GPU failures and slowdowns from the inside, using your CUDA softmax kernel as the starting point.

### Week 15: GPU architecture and the memory hierarchy

**Learn:** SMs, warps, global vs shared memory, coalescing, occupancy, roofline thinking.

**Resources:**

- Book: "Programming Massively Parallel Processors" (Hwu, Kirk, Hajj), chapters 1 to 6.
- NVIDIA CUDA C++ Programming Guide: https://docs.nvidia.com/cuda/cuda-c-programming-guide/
- NVIDIA CUDA Best Practices Guide: https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/
- GPU MODE lecture series (YouTube and the GPU MODE community).

### Week 16: Profile what you already built

**Do:** Profile your existing softmax kernel with Nsight Systems and Nsight Compute. Identify whether it is memory-bound or compute-bound. Optimize it step by step and record each change with numbers.

**Resources:**

- Nsight Systems: https://developer.nvidia.com/nsight-systems
- Nsight Compute: https://developer.nvidia.com/nsight-compute
- Simon Boehm, "How to Optimize a CUDA Matmul Kernel" (search the title, a standard walkthrough).
- Karpathy's llm.c: https://github.com/karpathy/llm.c

### Week 17: Attention and fused kernels

**Learn:** why attention is memory-bound, what FlashAttention changes, why fusion matters.

**Do:** Write a fused kernel in OpenAI Triton (the kernel language) and compare with the PyTorch version.

**Resources:**

- Paper: "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness" (arXiv 2205.14135)
- Triton tutorials: https://triton-lang.org/main/getting-started/tutorials/index.html
- CUTLASS: https://github.com/NVIDIA/cutlass

### Week 18: GPU memory and failure modes

**Do:** Reproduce and diagnose, using `compute-sanitizer`, `nvidia-smi`, PyTorch memory profiler, and vLLM logs:

- [ ] Out-of-memory from fragmentation vs real exhaustion
- [ ] Memory leak across requests
- [ ] Throughput drop from CPU-GPU transfer bottlenecks
- [ ] Underutilization from small batches

**Resources:**

- PyTorch Profiler and memory docs: https://pytorch.org/docs/stable/profiler.html
- Compute Sanitizer: https://docs.nvidia.com/compute-sanitizer/

### Week 19: Package GPU findings

**Do:** Publish the kernel repo with benchmarks and a write-up: "What the profiler told me about my own kernel."

**Posts:**

1. "My softmax kernel was 3x slower than PyTorch. Here's every step I took to close the gap."
2. "Memory-bound vs compute-bound: how to tell with one profiler run."
3. "GPU OOM is not always out of memory."

---

## Phase 5: Distributed systems for AI workloads (weeks 20 to 24)

**Goal:** Turn your Rust and distributed systems work into AI-specific infrastructure. Start flagship project 2 or 3.

### Weeks 20 to 21: Foundations (alongside the parallel track)

**Learn:** replication, consensus, failure detectors, idempotency, backpressure, exactly-once myths.

**Resources:**

- MIT 6.5840 Distributed Systems (lectures and labs, free): https://pdos.csail.mit.edu/6.824/
- Book: "Designing Data-Intensive Applications" (Kleppmann), the single most useful book here.
- Raft paper: https://raft.github.io/raft.pdf
- Fly.io Gossip Glomers challenges: https://fly.io/dist-sys/
- Jepsen analyses: https://jepsen.io

### Weeks 22 to 23: Build a queue-based inference platform

**Do:** Put a gateway and job queue in front of your inference workers. Implement timeouts, retries with backoff, idempotency keys, rate limiting, and load shedding. Add worker crash recovery. Use Rust for the gateway (tokio, axum, tonic) and Redis Streams, NATS, or Kafka for the queue.

**Resources:**

- Tokio tutorial: https://tokio.rs/tokio/tutorial
- Axum docs: https://docs.rs/axum
- Tonic: https://github.com/hyperium/tonic
- NATS docs: https://docs.nats.io
- Kafka docs: https://kafka.apache.org/documentation/
- Ray Serve docs (for comparison): https://docs.ray.io/en/latest/serve/index.html
- Google SRE Book chapters on handling overload and cascading failures (free, linked above).

### Week 24: Break it, then write it up

**Do:** Run retry-storm, thundering-herd, poison-message, and slow-consumer experiments. Show the metrics before and after your protections.

**Posts:**

1. "I added retries to my inference gateway and made an outage worse."
2. "Backpressure for LLM serving: what actually works."
3. "Crash recovery mid-generation: what state do you even need to save?"

---

## Phase 6: Consolidate and publish (weeks 25 to 26)

**Do:**

- [ ] Publish the long-form article for flagship project 1.
- [ ] Update your portfolio, GitHub pinned repos, and LinkedIn Featured section (best project, best article, GitHub, portfolio, best post).
- [ ] Write a 6-month review: what you built, what failed, numbers, what you misunderstood.
- [ ] Review analytics: median impressions, meaningful comments, follower growth.
- [ ] Only now consider adding the reliability angle on agents or RAG as a series.
- [ ] Update the headline once 3 to 5 public failure investigations exist.

---

## 10. Parallel track: Rust and systems programming

About 1.5 hours per week, every week. Use the **2024 edition** (`edition = "2024"` in `Cargo.toml`) for every new project.

|Weeks|Focus|Resource|
|---|---|---|
|1 to 4|Async Rust and tokio fundamentals|Tokio tutorial: https://tokio.rs/tokio/tutorial|
|5 to 10|Deepen your write-ahead log engine: crash recovery tests, fsync behavior, benchmarks|"Database Internals" (Alex Petrov)|
|11 to 16|Build a small load generator in Rust for inference APIs (open-loop, latency histograms)|`hdrhistogram` crate, `criterion` crate|
|17 to 22|Raft or replication experiments|`openraft` crate, MIT 6.5840 labs|
|23 to 26|Gateway for the inference platform|axum, tonic, tracing crates|

**Extra resources:**

- The Rust Book: https://doc.rust-lang.org/book/
- Jon Gjengset's YouTube channel and "Rust for Rustaceans".
- Linux debugging and performance: Brendan Gregg, "Systems Performance" (book) and https://www.brendangregg.com
- eBPF: bpftrace and bcc tutorials (find them in the iovisor GitHub org).

---

## 11. Master resource list

**Books**

- Designing Data-Intensive Applications, Martin Kleppmann
- Site Reliability Engineering and The SRE Workbook, Google (free online)
- Observability Engineering, Majors, Fong-Jones, Miranda
- Systems Performance, Brendan Gregg
- Programming Massively Parallel Processors, Hwu, Kirk, Hajj
- Database Internals, Alex Petrov
- Chaos Engineering, Rosenthal and Jones

**Papers (read in this order)**

1. The Tail at Scale (Dean and Barroso)
2. Orca (continuous batching)
3. PagedAttention (vLLM)
4. FlashAttention
5. Raft
6. Attention Is All You Need

**Courses and video**

- Karpathy: Intro to LLMs, Let's build GPT, llm.c
- MIT 6.5840 Distributed Systems
- GPU MODE lectures
- Jon Gjengset (Rust)

**Official docs worth bookmarking**

- vLLM, SGLang, Triton Inference Server, TensorRT-LLM
- Prometheus, Grafana, OpenTelemetry, DCGM
- Kubernetes, KServe, KEDA, NVIDIA GPU Operator
- CUDA Programming Guide, Nsight tools

**Hands-on challenges**

- Fly.io Gossip Glomers
- MIT 6.5840 labs
- GPU MODE kernel challenges

---

## 12. Rules to keep this roadmap honest

1. **Build before you read more.** If a week has more reading than building, you are procrastinating.
2. **Every experiment gets a note.** Setup, numbers, surprise. No note, it did not happen.
3. **One post per finding.** The roadmap and the content calendar are the same plan.
4. **Cost cap.** Set a monthly GPU budget and shut instances down after every session.
5. **Do not add tools mid-phase.** If a new tool tempts you, put it on a "later" list.
6. **Publish the failures.** A benchmark where you misconfigured something and fixed it is worth more than a clean result.
7. **Stay inside the line.** Each topic must answer: how does this fail, how would I detect it, and why?

---

## 13. First 7 days (do this now)

- [ ] Day 1: Create the repo and `notes/` folder. Set a GPU budget.
- [ ] Day 2: Watch Karpathy's Intro to LLMs. Write your "what happens during one request" note.
- [ ] Day 3: Install Docker and kind. Run a small model with llama.cpp.
- [ ] Day 4: Create a GPU account (RunPod, Lambda, or Modal) and confirm `nvidia-smi` works.
- [ ] Day 5: Read the kipply inference arithmetic article.
- [ ] Day 6: Compute the KV cache size for a 7B model on paper.
- [ ] Day 7: Publish the "6-month public roadmap" post.