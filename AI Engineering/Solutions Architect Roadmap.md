# Study resources by topic

I'm giving names rather than raw URLs because links and course catalogs shift often. Search the exact title and verify it's current, especially for NVIDIA tools, which change fast.

## Start here (the 6 highest-leverage items)

1. **Andrej Karpathy, "Neural Networks: Zero to Hero"** (YouTube), especially "Let's build GPT"
2. **vLLM paper (PagedAttention)** plus the vLLM docs
3. **GPU MODE lecture series** (YouTube) plus **Programming Massively Parallel Processors**
4. **"How to Scale Your Model"** (Google DeepMind online book, free)
5. **NVIDIA GPU Operator and MIG docs** on a real cluster
6. **TensorRT-LLM and Triton docs**, deployed and benchmarked by you

## 1. LLM and DL inference theory

|Topic|Where to study|
|---|---|
|Transformer architecture, attention|"Attention Is All You Need" (arXiv 1706.03762), Jay Alammar's "The Illustrated Transformer", Karpathy's "Let's build GPT"|
|KV cache, prefill vs decode, memory-bound vs compute-bound|kipply's "Transformer Inference Arithmetic" blog, Horace He's "Making Deep Learning Go Brrrr From First Principles", the "How to Scale Your Model" book|
|Roofline thinking|"How to Scale Your Model" (rooflines chapter), the original Roofline paper (Williams et al.)|
|Continuous batching|Orca paper (OSDI 2022), Anyscale's blog on continuous batching|
|PagedAttention, prefix caching|vLLM paper (arXiv 2309.06180), vLLM docs and source code|
|Chunked prefill|Sarathi-Serve paper|
|Speculative decoding|Leviathan et al. "Fast Inference from Transformers via Speculative Decoding", Medusa and EAGLE papers|
|FlashAttention|FlashAttention 1, 2, and 3 papers, Tri Dao's talks|
|Quantization|AWQ and GPTQ papers, FP8 formats whitepaper (NVIDIA, Arm, Intel), Hugging Face quantization docs, TensorRT Model Optimizer docs|
|Parallelism (TP, PP, EP, DP)|Megatron-LM paper, Hugging Face "Ultra-Scale Playbook", Stas Bekman's `ml-engineering` GitHub repo|
|Metrics (TTFT, ITL, goodput)|NVIDIA's "LLM Inference Benchmarking" blog series, DistServe paper (defines goodput)|
|Survey of everything|"Efficient LLM Serving" survey papers on arXiv, Lilian Weng's blog posts on inference optimization|

## 2. Deep learning frameworks and Python

- **PyTorch**: official tutorials, then "Deep Learning with PyTorch" (Stevens et al.), then read the PyTorch internals blog by Edward Yang (ezyang)
- **PyTorch Profiler** docs and the PyTorch performance tuning guide
- **Hugging Face**: the LLM Course (free), Transformers docs, and the Accelerate library docs
- **TensorFlow**: just the official guides, enough to read existing code
- **Python depth**: "Fluent Python" (Ramalho), "High Performance Python" (Gorelick), asyncio docs, and the Real Python profiling guides

## 3. CUDA, GPU architecture, and profiling

- **Programming Massively Parallel Processors** (Hwu, Kirk, Hajj): the standard textbook
- **GPU MODE** lectures and community (YouTube and Discord)
- **NVIDIA CUDA C++ Programming Guide** and **CUDA Best Practices Guide** (reference, not cover to cover)
- **NVIDIA Deep Learning Institute**: "Fundamentals of Accelerated Computing with CUDA" and the Nsight-focused courses
- **Nsight Systems and Nsight Compute** official docs, plus the NVIDIA developer blog profiling walkthroughs
- **GPU architecture**: NVIDIA's Hopper and Blackwell architecture whitepapers, and Chips and Cheese for independent analysis
- **Practice idea**: write a softmax or layernorm kernel, wrap it as a PyTorch extension, and benchmark it against the built-in version

## 4. NVIDIA inference stack

|Tool|Where to study|
|---|---|
|TensorRT|TensorRT developer guide and samples on GitHub|
|TensorRT-LLM|GitHub repo (NVIDIA/TensorRT-LLM) docs and examples, plus the NVIDIA technical blog|
|Triton Inference Server|Triton docs, `perf_analyzer` and Model Analyzer docs, the tutorials repo on GitHub|
|NIM|NVIDIA NIM docs and the build.nvidia.com catalog|
|Dynamo|NVIDIA Dynamo GitHub and docs, GTC talks on disaggregated serving|
|vLLM and SGLang (comparison)|Their docs and GitHub, plus the SGLang and RadixAttention paper|
|GenAI-Perf and benchmarking|NVIDIA docs for GenAI-Perf (check whether a newer tool has replaced it), MLPerf Inference results and methodology on mlcommons.org|

**GTC session recordings** (free on NVIDIA On-Demand) are the best way to see how NVIDIA's own engineers frame these tools.

## 5. Kubernetes, GPU orchestration, and containers

- **Kubernetes**: official docs, "Kubernetes in Action" (Lukša), "Kubernetes the Hard Way" (Kelsey Hightower)
- **Certification** (optional but a useful forcing function): CKA, then CKAD
- **NVIDIA GPU Operator**, **Container Toolkit**, and **device plugin** docs
- **MIG User Guide** (NVIDIA) and the GPU Operator's MIG sections
- **Time-slicing vs MIG vs MPS**: NVIDIA's GPU sharing docs for Kubernetes
- **Scheduling**: Kubernetes docs on affinity, taints, topology manager, plus Kueue and Volcano project docs
- **Helm** docs and the Operator SDK / Kubebuilder book for CRDs
- **KServe** and **KubeRay** docs, since many customers serve LLMs through them

## 6. Observability and power efficiency

- **DCGM and DCGM Exporter** docs (NVIDIA)
- **Prometheus** and **Grafana** official docs and tutorials
- **OpenTelemetry** docs
- **"Observability Engineering"** (Majors, Fong-Jones, Miranda) and the Google SRE book (you likely know the latter)
- **Power analysis**: NVML docs, `nvidia-smi` power queries, DCGM power metrics, and MLPerf Power methodology

## 7. Systems, C/C++, and performance

- **"Computer Systems: A Programmer's Perspective"** (CS:APP)
- **"Systems Performance"** by Brendan Gregg, plus his flamegraph and `perf` pages
- **C++**: "A Tour of C++" (Stroustrup), cppreference, and Compiler Explorer for inspecting generated code
- **Debugging and profiling tools**: gdb, valgrind, `perf` man pages and tutorials
- **Parallel programming**: "An Introduction to Parallel Programming" (Pacheco), OpenMP and MPI tutorials from LLNL (hpc-tutorials.llnl.gov)
- **Distributed computing**: "Designing Data-Intensive Applications" (Kleppmann), MIT 6.824 lectures
- **NCCL and interconnects**: NCCL docs, the `nccl-tests` repo, NVIDIA's NVLink and InfiniBand whitepapers

## 8. Customer-facing and communication skills

- **Google Technical Writing courses** (free, developers.google.com)
- **"The Pyramid Principle"** (Barbara Minto) for structuring recommendations
- **"Made to Stick"** (Heath brothers) and **"Resonate"** (Nancy Duarte) for presentations
- **"Crucial Conversations"** for handling disagreement
- **Learn from the source**: watch GTC talks by NVIDIA solutions architects and study how they handle Q&A
- **Practice loop**: write a benchmark report, post it publicly, present it at a meetup or internal talk, and repeat
- **Julia Evans's blog and zines** as a model for explaining hard technical things simply

## Suggested weekly rhythm

- **2 sessions** reading or watching theory
- **2 sessions** hands-on (kernels, deployments, benchmarks)
- **1 session** writing up what you learned, since the writing is the proof-of-work for this role

Without hands-on benchmarks, this list is just reading. Every topic above should end with something you ran and measured.

---
# 26-week schedule

**Assumptions:** about 8 to 10 hours a week alongside your internship and final year, in the 5-session rhythm from before (2 theory, 2 hands-on, 1 write-up). If you start the week of Oct 5, week 26 lands around early April 2027.

**GPU access is the main logistics question.** Weeks 1 to 2 need no GPU. From week 3 onward, use Colab or a cheap hourly cloud GPU (RunPod, Lambda, Vast) if you have no local card. MIG (week 22) needs an A100 or H100-class card, so budget a few rented hours for that week only.

**Rule:** if a week slips, push everything back. Do not skip the "Ship" column. Your CUDA project plan of one stage per session, not moving on until it is understood, applies to the whole schedule.

## Phase A: Foundations (weeks 1 to 4)

|Wk|Study|Ship|
|---|---|---|
|1|Karpathy "Zero to Hero" (micrograd, makemore), official PyTorch tutorials|Notebook with your own training loop|
|2|"Let's build GPT", Illustrated Transformer, "Attention Is All You Need"|Small GPT trained, with hand-counted parameters and FLOPs per token|
|3|kipply "Transformer Inference Arithmetic", Horace He "Brrrr" post, PMPP ch. 1 to 3. **CUDA stage 1**|Vector add running on GPU, plus a one-page note on why decode is memory-bound|
|4|PMPP ch. 4 to 5, GPU MODE lectures 1 to 2. **CUDA stage 2**|Naive softmax kernel, correct against `torch.softmax`|

## Phase B: CUDA depth (weeks 5 to 8)

|Wk|Study|Ship|
|---|---|---|
|5|PMPP reductions chapter. **CUDA stage 3** (shared memory, coalescing, warp reductions)|Optimized softmax, with a note on what each optimization bought|
|6|PyTorch C++/CUDA extension docs. **Stages 4 and 5**|Extension wrapper plus a benchmark across sizes|
|7|Nsight Systems and Nsight Compute docs, FlashAttention paper. **Stage 6**|Profile report with bottleneck identified|
|8|"How to Scale Your Model" rooflines chapter. **Stage 7**|Published write-up #1 (blog or GitHub). Stretch: Triton kernel comparison|

## Phase C: Inference engine internals (weeks 9 to 13)

|Wk|Study|Ship|
|---|---|---|
|9|vLLM paper (PagedAttention), vLLM docs|Small model served with vLLM, OpenAI-compatible endpoint working|
|10|Orca paper, Anyscale continuous batching post, DistServe (goodput)|Load test measuring TTFT, inter-token latency, throughput vs concurrency|
|11|AWQ and GPTQ papers, FP8 formats whitepaper, HF quantization docs|Same model at FP16 vs FP8/AWQ: accuracy vs latency table|
|12|Speculative decoding, Medusa/EAGLE, Sarathi-Serve (chunked prefill), prefix caching docs|Toggle each feature in vLLM and measure the effect|
|13|SGLang and RadixAttention paper|SGLang vs vLLM benchmark. Published write-up #2|

## Phase D: NVIDIA stack (weeks 14 to 19)

|Wk|Study|Ship|
|---|---|---|
|14|TensorRT developer guide, TensorRT-LLM docs and examples|Engine built for a small model|
|15|TensorRT-LLM in-flight batching, quantization via Model Optimizer|TRT-LLM vs vLLM benchmark on identical hardware|
|16|Triton docs, tutorials repo, `perf_analyzer`|Model served via Triton with a TRT-LLM backend|
|17|NIM docs, build.nvidia.com, GenAI-Perf (check for a replacement)|NIM container deployed and benchmarked|
|18|Dynamo docs, GTC talks on disaggregated serving|Architecture diagram and notes explaining prefill/decode disaggregation and KV-aware routing|
|19|HF "Ultra-Scale Playbook", Megatron-LM paper, NCCL docs|`nccl-tests` run if you can rent multi-GPU for an hour, otherwise a written TP vs PP vs EP comparison|

## Phase E: Kubernetes and observability (weeks 20 to 24)

Your SRE internship and experiment server help most in this phase, so start with what they already give you.

|Wk|Study|Ship|
|---|---|---|
|20|K8s docs on scheduling, affinity, taints, topology manager. CKA prep begins|Written checklist of gaps vs the CKA syllabus|
|21|GPU Operator, Container Toolkit, device plugin docs|GPU node on k3s or a managed cluster running a CUDA pod|
|22|MIG User Guide, time-slicing vs MIG vs MPS|MIG-partitioned GPU, workloads scheduled onto slices, comparison table of the three sharing modes|
|23|DCGM Exporter, Prometheus, Grafana, NVML power docs|Dashboard with utilization, memory, SM activity, power draw|
|24|KServe or KubeRay docs, Helm|vLLM or Triton deployed on K8s, load tested, with performance-per-watt analysis|

## Phase F: Communication and positioning (weeks 25 to 26)

|Wk|Study|Ship|
|---|---|---|
|25|Google Technical Writing course, Pyramid Principle|Reference-architecture report combining weeks 9 to 24 into one customer-style recommendation|
|26|Duarte "Resonate", Q&A practice|A 20-minute talk given at a meetup, internal session, or recorded, plus a public post|

## Checkpoints

- **Week 8:** CUDA kernel and write-up #1 public. If this slipped past week 10, the schedule is too heavy and you should cut Phase D scope, not the write-ups.
- **Week 13:** you can explain KV cache, continuous batching, and quantization tradeoffs without notes, and you have measured each one.
- **Week 19:** you have benchmark numbers from vLLM, SGLang, and TRT-LLM on the same hardware.
- **Week 26:** three public write-ups, one talk, one K8s GPU deployment with dashboards.

## Blunt reality check

Finishing this makes you a credible candidate for ML platform and inference infrastructure roles, not for the Solutions Architect role itself. That role still wants years of production experience, so the portfolio is what gets you the stepping-stone job. Use your internship to get any GPU or ML-serving exposure you can. Even helping a team that runs inference workloads counts for more than another solo project.

I can turn any single phase into a day-by-day plan with exact reading sections, or adjust the schedule once you know your GPU access situation.