Yes. And I’d change the strategy.

You don't want papers where you merely **implement the idea**. You want papers where you can:

1. Understand the algorithm/system.
2. Reproduce an experiment or result.
3. Compare your numbers against the paper.
4. Put code, benchmarks, graphs, and a reproduction report on GitHub.

For your interests, these are the best candidates.

|Order|Paper|What you reproduce|Difficulty|GitHub value|
|---|---|---|---|---|
|**1**|**Raft**|Leader election, replication, failures, latency|⭐⭐|🔥🔥🔥|
|**2**|**MapReduce**|Distributed word count/indexing + scaling|⭐⭐|🔥🔥🔥|
|**3**|**Dynamo**|Consistent hashing, replication, quorum reads/writes|⭐⭐⭐|🔥🔥🔥|
|**4**|**Dapper**|Distributed tracing + request latency analysis|⭐⭐⭐|🔥🔥🔥|
|**5**|**The Tail at Scale**|p50/p95/p99 latency and tail amplification|⭐⭐⭐|🔥🔥🔥|
|**6**|**Bitcask**|Log-structured KV store + performance comparison|⭐⭐⭐|🔥🔥|
|**7**|**Kafka**|Partitioning, replication and throughput experiments|⭐⭐⭐|🔥🔥🔥|
|**8**|**GFS**|Chunking, replication and failure recovery|⭐⭐⭐⭐|🔥🔥🔥|
|**9**|**Spanner**|Simplified distributed transactions / clock model|⭐⭐⭐⭐|🔥🔥🔥|
|**10**|**Borg**|Simplified scheduler + workload experiments|⭐⭐⭐⭐|🔥🔥🔥|
|**11**|**LoRA**|Fine-tuning vs full fine-tuning, memory/performance|⭐⭐⭐|🔥🔥🔥|
|**12**|**QLoRA**|4-bit quantization + LoRA vs baselines|⭐⭐⭐⭐|🔥🔥🔥|
|**13**|**FlashAttention**|Attention implementations + memory/runtime|⭐⭐⭐⭐|🔥🔥🔥|
|**14**|**vLLM / PagedAttention**|KV-cache management + serving throughput|⭐⭐⭐⭐⭐|🔥🔥🔥|
|**15**|**DistServe**|Prefill/decode disaggregation + serving latency|⭐⭐⭐⭐⭐|🔥🔥🔥|

### I'd start with these 5

**1. Raft**

Build a 3/5-node cluster in Go or Rust.

Then reproduce:

- leader election
- node failure
- network partitions
- log replication
- recovery
- throughput vs number of nodes
- latency under failure

This is particularly strong because the paper gives you concrete algorithmic behavior to validate, and there are established implementations and testing approaches you can study without simply copying them. [GitHub](https://github.com/nyu-distributed-systems/sp25-raft-lab?utm_source=chatgpt.com)

**Repository:**

```
raft-reproduction/
├── src/
├── tests/
├── benchmarks/
├── experiments/
├── results/
│   ├── latency.png
│   ├── throughput.png
│   └── failure-recovery.png
├── paper-notes.md
└── README.md
```

---

**2. MapReduce**

Implement your own mini MapReduce framework.

Experiment with:

```
Workers:       1  2  4  8  16
Dataset size:  1GB 5GB 10GB
Measure:       runtime, throughput, scalability
```

Then compare your scaling curve with the paper.

There are already student implementations demonstrating that MapReduce and Raft are very practical reproduction projects. [GitHub](https://github.com/LukaDarsalia/distributed_systems?utm_source=chatgpt.com)

---

**3. Dynamo**

This would be excellent for your backend/distributed-database interests.

Implement:

```
Consistent Hash Ring
        ↓
Replication
        ↓
Quorum Reads/Writes
        ↓
Failure Injection
        ↓
Consistency experiments
```

Then measure:

- availability
- read/write latency
- replication factor
- quorum size
- node failures

This becomes a legitimate **distributed database project**, not just a toy CRUD application.

---

**4. The Tail at Scale**

This one is underrated for your portfolio.

Build a service where requests fan out:

```
             ┌── Service A
Request ─────┼── Service B
             ├── Service C
             └── Service D
```

Measure p50/p95/p99 latency as the number of downstream services increases.

Then reproduce the paper's central observation: **small amounts of tail latency at individual services can become significant at the end-to-end request level.**

This gives you beautiful graphs for GitHub.

---

**5. vLLM / PagedAttention**

Save this for later.

Build a simplified KV-cache manager and compare:

```
Naive KV cache
       vs
Paged KV cache
```

Measure:

- GPU memory utilization
- concurrent requests
- throughput
- latency
- maximum supported sequence/request count

This connects your **LLM + systems + GPU + distributed infrastructure** interests extremely well.

---

### One important distinction

Don't put:

> "Implemented Raft."

on your resume.

That's weak.

Put something like:

> **Reproduced Raft consensus experiments** by implementing a fault-tolerant replicated state machine, injecting node/network failures, and benchmarking election recovery, replication latency, and throughput across 3-7 node clusters.

That's substantially stronger because you've demonstrated **research reproduction + systems engineering + experimental methodology**.

And your GitHub should contain **the paper, implementation, experiment methodology, raw results, graphs, and a reproduction report**, not just source code.

If I were optimizing specifically for your profile, I'd make your first sequence:

**Raft → MapReduce → Dynamo → Tail at Scale → Dapper → Kafka → LoRA → QLoRA → FlashAttention → vLLM**

That gives you a very coherent **Distributed Systems → Infrastructure → AI Systems** research portfolio.