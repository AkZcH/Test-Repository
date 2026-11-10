This replaces the generic version. It reweights your 10 quiz topics for an AI Infrastructure role and adds the big areas the quiz did not cover. Those missing areas (inference serving, GPU programming, cluster scheduling) are where FAANG AI infra interviews spend most of their time.

URLs are written from memory. If one has moved, search the title.

---

## 1. What the interview loop usually looks like

Exact loops vary by company and level, but expect roughly these rounds:

|Round|What it tests|Your prep emphasis|
|---|---|---|
|Coding (1 to 2 rounds)|Data structures, algorithms, clean code|Maintain LeetCode practice, do not let it decay|
|Systems / infra deep dive|GPUs, networking, distributed training, Linux, Kubernetes|Sections 3 to 7 below|
|ML systems design|Design an inference platform, a training platform, a feature or vector store|Section 8|
|Debugging / incident scenario|Quiz-style questions: symptom, mechanism, fix|Your original 10 topics|
|Behavioral|Ownership, ambiguity, conflict, impact|Section 9|

Honest note on "FAANG level": the bar differs a lot between new grad, mid-level, and senior. At new grad or early level, they weight coding and fundamentals heavily and judge your projects for depth. At senior level, they expect production war stories and design leadership. Tell me which level and I will adjust. Until then this guide assumes early-career with strong projects.

---

## 2. Priority of your original 10 quiz topics

|Priority|Quiz topic|Why|
|---|---|---|
|Tier 1 (core)|Q1 GPU/NCCL/InfiniBand training, Q9 vector DB and quantization|Directly your job domain|
|Tier 2 (expected)|Q2 K8s graceful shutdown, Q5 eBPF/mesh, Q7 Raft/consensus|Standard infra depth, frequently used as differentiators|
|Tier 3 (know the basics)|Q3 Terraform state, Q4 Linux df/du, Q6 ArgoCD/Rollouts, Q8 OIDC, Q10 BGP/BFD|Common SRE questions, rarely the main event for AI infra. Q4 (Linux) is the exception: it is cheap to learn and gets asked everywhere|

Use the existing guide (`sre-interview-study-resources.md`) for Tier 2 and Tier 3 details. The rest of this file covers Tier 1 in more depth and the new areas.

---

## 3. GPU architecture and programming (new, high priority)

**Why it matters:** You cannot reason about inference or training performance without knowing where time goes: compute-bound vs memory-bandwidth-bound, occupancy, memory hierarchy.

**Core concepts**

- SMs, warps, thread blocks, shared memory, registers, L2, HBM
- Roofline model, arithmetic intensity, why decode is memory-bound and prefill is compute-bound
- Tensor cores, precision formats (FP32, TF32, BF16, FP16, FP8), accumulation
- Memory coalescing, bank conflicts, kernel fusion
- Streams, async copies, CUDA graphs
- NVLink, NVSwitch, PCIe, GPUDirect RDMA

**Resources**

- Horace He, "Making Deep Learning Go Brrrr From First Principles": https://horace.io/brrr_intro.html
- CUDA C++ Programming Guide: https://docs.nvidia.com/cuda/cuda-c-programming-guide/
- _Programming Massively Parallel Processors_, Hwu, Kirk, Hajj (book)
- GPU MODE lecture series: https://github.com/gpu-mode/lectures
- Triton: https://triton-lang.org
- FlashAttention paper: https://arxiv.org/abs/2205.14135 and FlashAttention-2: https://arxiv.org/abs/2307.08691
- Stanford CS149 (Parallel Computing) and CMU 15-418: search the course names for current pages

**Lab:** Extend your softmax kernel work: write a fused softmax, profile with Nsight Compute, and explain in roofline terms why it is memory-bound. Then write a short naive vs tiled matmul and report the speedup.

**Be ready to answer**

- Why is LLM decode memory-bandwidth-bound, and what does batching do to that?
- What does FlashAttention change and why does it not change the math?
- How would you profile a slow training step? (Nsight Systems for timeline, Nsight Compute for kernels, PyTorch profiler for framework view.)

---

## 4. LLM inference serving (new, highest priority)

This is the single most likely area for a deep-dive round. Know it cold.

**Core concepts**

- Prefill vs decode, time to first token (TTFT), inter-token latency, throughput
- KV cache: size formula, memory growth, why it dominates memory
- Continuous (in-flight) batching, PagedAttention, prefix caching
- Speculative decoding, chunked prefill
- Quantization: weight-only (AWQ, GPTQ), INT8, FP8, KV cache quantization
- Parallelism for inference: tensor parallel, pipeline, expert parallel for MoE
- Prefill/decode disaggregation
- Autoscaling GPU inference, cold start, model loading time, request routing
- SLOs: p50/p99 latency, goodput, cost per million tokens

**Resources**

- vLLM paper (PagedAttention): https://arxiv.org/abs/2309.06180 and docs: https://docs.vllm.ai
- Kipply, "Transformer Inference Arithmetic": https://kipp.ly/transformer-inference-arithmetic/
- Lilian Weng, "Large Transformer Model Inference Optimization": https://lilianweng.github.io/posts/2023-01-10-inference-optimization/
- Speculative decoding: https://arxiv.org/abs/2211.17192
- DistServe (disaggregation): https://arxiv.org/abs/2401.09670
- Splitwise: https://arxiv.org/abs/2311.18677
- AWQ: https://arxiv.org/abs/2306.00978, GPTQ: https://arxiv.org/abs/2210.17323
- TensorRT-LLM docs: https://nvidia.github.io/TensorRT-LLM/
- SGLang: https://github.com/sgl-project/sglang
- Orca paper (continuous batching, OSDI 2022): search the title

**Lab:** Serve a 7B or 8B model with vLLM, then benchmark throughput and latency under load at different batch sizes and max concurrency. Plot TTFT and tokens per second. Compute the KV cache size by hand and check it against what vLLM reports.

**Be ready to answer**

- Compute KV cache per token for a model given layers, KV heads, head dim, and dtype.
- Why does continuous batching beat static batching?
- A service has good average latency but bad p99. List five causes and how you would tell them apart.
- How would you cut cost per token by 2x without hurting quality?

---

## 5. Distributed training (expands Q1)

**Core concepts**

- Data, tensor, pipeline, sequence, and expert parallelism, and 3D/4D combinations
- ZeRO stages 1 to 3, FSDP, activation checkpointing, mixed precision
- Communication cost model: bytes moved per step, overlap of compute and comms
- Checkpointing: sharded and async checkpoints, restart time, goodput
- Failure at scale: stragglers, silent data corruption, NIC or GPU faults, elastic training
- Metrics: MFU (model FLOPs utilization), step time breakdown

**Resources**

- "How to Scale Your Model" (JAX scaling book, excellent regardless of framework): https://jax-ml.github.io/scaling-book/
- Hugging Face Ultra-Scale Playbook: https://huggingface.co/spaces/nanotron/ultrascale-playbook
- Megatron-LM: https://arxiv.org/abs/2104.04473
- ZeRO: https://arxiv.org/abs/1910.02054, PyTorch FSDP: https://arxiv.org/abs/2304.11277
- GPipe: https://arxiv.org/abs/1811.06965
- Llama 3 paper (infrastructure and failure statistics): https://arxiv.org/abs/2407.21783
- PyTorch distributed checkpointing: https://pytorch.org/docs/stable/distributed.checkpoint.html
- torchrun / elastic: https://pytorch.org/docs/stable/elastic/run.html
- Stas Bekman, _ML Engineering_: https://github.com/stas00/ml-engineering

**Lab:** Train a small GPT with DDP, then FSDP, on 2+ GPUs (rent cheap ones). Log step time, memory, and MFU. Break it: kill a rank mid-run and implement resume from a sharded checkpoint.

**Be ready to answer**

- Why does 90% GPU utilization not mean the job is healthy? (Quiz Q1: utilization counts busy time, not useful work. Stalls in collectives can still show as "busy".)
- At 16K GPUs, failures happen constantly. What do you design for?
- Compare FSDP and tensor parallelism: when does each win?

---

## 6. Cluster scheduling and orchestration (new)

**Core concepts**

- Kubernetes for GPUs: device plugin, NVIDIA GPU Operator, MIG, time-slicing, Dynamic Resource Allocation
- Gang scheduling and queueing: Kueue, Volcano, Slurm
- Topology-aware placement (same NVLink domain, same leaf switch, same rail)
- Fair share, preemption, priorities, quotas, bin packing vs fragmentation
- Ray for distributed workloads
- Health checking: DCGM, XID errors, node draining, burn-in tests
- Observability: GPU metrics, NCCL timing, job-level goodput

**Resources**

- NVIDIA GPU Operator: https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/
- DCGM: https://docs.nvidia.com/datacenter/dcgm/latest/
- Kueue: https://kueue.sigs.k8s.io/
- Volcano: https://volcano.sh/
- Kubernetes DRA: https://kubernetes.io/docs/concepts/scheduling-eviction/dynamic-resource-allocation/
- Slurm docs: https://slurm.schedmd.com/documentation.html
- Ray docs: https://docs.ray.io

**Lab:** On a kind or small cloud cluster, install Kueue, submit jobs that need 4 "GPUs" worth of quota, and watch queueing, preemption, and fairness behavior.

**Be ready to answer**

- Why does default Kubernetes scheduling fail for distributed training? (No gang scheduling: a job can get 3 of 4 pods and deadlock holding resources.)
- How do you keep GPU fragmentation low in a shared cluster?
- How would you detect and quarantine a flaky GPU node automatically?

---

## 7. Networking, storage, and data path (expands Q1, Q5, Q10)

**Core concepts**

- RDMA, InfiniBand vs RoCEv2, congestion control (ECN, PFC, DCQCN), adaptive routing
- Fat-tree and rail-optimized topologies, oversubscription
- NCCL algorithms, protocols, environment variables, debugging
- Storage for training: parallel filesystems, object storage, data loaders, caching
- Checkpoint storage bandwidth as a bottleneck
- eBPF for observability and networking (Q5), Raft for metadata and control planes (Q7)

**Resources**

- NCCL docs and `nccl-tests` (see earlier guide)
- Meta Engineering, "Building Meta's GenAI Infrastructure" and the RoCE network posts: search titles on engineering.fb.com
- Llama 3 paper network sections (above)
- Ethernet vs InfiniBand for AI clusters: search recent NVIDIA and Meta write-ups

**Be ready to answer**

- Explain PFC and why it can cause head-of-line blocking or deadlock.
- How do you find which link is the bad one in a 1000-node fabric?

---

## 8. ML systems design practice

Practice answering these out loud in 35 to 45 minutes each. Use a fixed structure: requirements and scale, API, high-level architecture, deep dives on 2 components, bottlenecks, failure modes, cost, metrics.

1. Design a multi-tenant LLM inference platform serving many models with different SLOs.
2. Design a training platform for 10K-GPU jobs: scheduling, checkpointing, fault tolerance.
3. Design a vector search service for 1B embeddings at p99 under 50 ms (Quiz Q9, expanded: sharding, replication, filtered search, index rebuilds, memory budget).
4. Design a GPU fleet health and observability system.
5. Design a model artifact distribution system (weights are 100s of GB, thousands of nodes need them fast).
6. Design a batch inference pipeline for 1B documents with a cost target.

**Resources**

- Chip Huyen, _Designing Machine Learning Systems_ and _AI Engineering_ (books)
- _Designing Data-Intensive Applications_ for replication, partitioning, consensus
- Stanford CS336, "Language Modeling from Scratch" (systems lectures): search the course page

---

## 9. Behavioral and positioning

- Prepare 6 stories using situation, action, result with real numbers: a hard debugging session, a disagreement, a time you owned something ambiguous, a failure, a time you learned fast, a time you improved a system.
- Be able to explain every project at three depths: 1 minute, 5 minutes, deep dive.
- Know the company's published infra work (engineering blogs, papers) and reference it naturally.

**Mapping your existing projects to interview talking points**

- CUDA softmax kernel: your GPU programming and profiling story. Push it further (fusion, benchmarks vs `torch.softmax`, roofline reasoning).
- Weir (distributed rate limiter): systems design, observability, benchmarking, tail latency.
- Ferrux (Rust write-ahead log): durability, fsync semantics, consensus adjacency. Raft would be the natural next step.
- Cairn (RAG system): vector search, quantization, evaluation. Be ready to defend index and quantization choices with measured recall and memory numbers.

Direct feedback: a portfolio of five unfinished projects reads worse than two finished, benchmarked, well-documented ones. For this role, I would finish the CUDA project with real profiling data and one inference-serving benchmark project, and keep the rest as supporting evidence.

---

## 10. 10-week plan

|Weeks|Focus|Deliverable|
|---|---|---|
|1 to 2|GPU architecture, roofline, CUDA profiling|Fused kernel with Nsight report and written analysis|
|3 to 4|Inference serving deep dive|vLLM benchmark write-up with KV cache math and latency plots|
|5 to 6|Distributed training and NCCL|DDP vs FSDP experiment, checkpoint-resume demo|
|7|Scheduling and K8s for GPUs, Raft|Kueue lab, Raft lab progress|
|8|Networking, storage, Tier 3 quiz topics refresh|One-page incident cards|
|9|ML systems design mocks (2 per week)|Recorded or peer-reviewed designs|
|10|Full mock loops, behavioral stories|Timed mock interviews|

Throughout all weeks: 3 to 5 coding problems per week to stay sharp, and one 60-second verbal explanation per day of a concept from the guide.

---

## 11. Recurring rapid-fire questions to drill

- Why is decode memory-bound? Compute arithmetic intensity for a matvec.
- What is MFU and what is a good number? (Roughly 40 to 55 percent is strong for large dense training.)
- AllReduce volume per GPU for ring AllReduce on N GPUs? (About 2(N-1)/N times the data size.)
- What happens on a GPU XID 79 or an uncorrectable ECC error?
- Why can a single slow GPU slow a synchronous job, and how do you detect stragglers?
- HNSW vs IVF-PQ vs DiskANN: memory, recall, latency, update cost.
- How does a pod get a GPU, end to end, in Kubernetes?
- What breaks if two ranks disagree on collective order? (Hang or deadlock, a classic NCCL debugging story.)