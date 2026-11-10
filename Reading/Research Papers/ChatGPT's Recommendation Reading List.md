Based on your interests, I would **not** make this an AI-only reading list. Your strongest intersection is **distributed systems + cloud infrastructure + systems engineering + AI infrastructure**.

I’d read them in this order, because each paper builds concepts needed for the next.

### Tier 1: Foundations of distributed systems

1. **MapReduce: Simplified Data Processing on Large Clusters**  
    Dean & Ghemawat, 2004  
    Learn distributed computation, partitioning, scheduling, fault tolerance.
    
2. **The Google File System (GFS)**  
    Learn distributed storage, replication, failure handling.
    
3. **Dynamo: Amazon’s Highly Available Key-value Store**  
    Learn eventual consistency, quorum systems, consistent hashing.
    
4. **Bigtable: A Distributed Storage System for Structured Data**  
    Extremely relevant to your backend/database interests. [Google Research](https://research.google/pubs/bigtable-a-distributed-storage-system-for-structured-data/?utm_source=chatgpt.com)
    
5. **The Chubby Lock Service for Loosely-Coupled Distributed Systems**  
    Learn distributed coordination and locking.
    
6. **ZooKeeper: Wait-free Coordination for Internet-scale Systems**  
    Understand how distributed services coordinate.
    

### Tier 2: Consensus and distributed databases

7. **Paxos Made Simple**  
    Lamport.  
    You need to understand consensus properly.
    
8. **In Search of an Understandable Consensus Algorithm (Raft)**  
    One of the most important papers for you. Learn leader election, replication and consensus.
    
9. **Spanner: Google's Globally Distributed Database**  
    A must-read for someone interested in cloud/distributed infrastructure. It combines replication, transactions, sharding and global distribution. [Google Research](https://research.google/pubs/spanner-googles-globally-distributed-database-2/?utm_source=chatgpt.com)
    
10. **Spanner: Becoming a SQL System**  
    Goes deeper into distributed query execution and database architecture. [Google Research](https://research.google/pubs/spanner-becoming-a-sql-system/?hl=id&utm_source=chatgpt.com)
    
11. **Calvin: Fast Distributed Transactions for Partitioned Database Systems**  
    Distributed transactions and deterministic execution.
    

### Tier 3: Cloud infrastructure

12. **Large-scale Cluster Management at Google with Borg**  
    Extremely relevant to Kubernetes and cloud infrastructure. [Google Research](https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/?utm_source=chatgpt.com)
    
13. **Borg, Omega, and Kubernetes**  
    Understand the lineage of modern container orchestration.
    
14. **Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center**  
    Cluster resource management.
    
15. **The Datacenter as a Computer**  
    Think about infrastructure at warehouse scale.
    
16. **Jupiter Rising: A Decade of Clos Topologies and Centralized Control in Google's Datacenter Network**  
    Networking at cloud scale.
    

### Tier 4: Systems and reliability

17. **The Tail at Scale**  
    One of the most important papers for understanding latency in distributed systems.
    
18. **The Morning Paper: Everything You Ever Wanted to Know About Distributed Systems**  
    Use this as a broad conceptual bridge before going deeper.
    
19. **Impossibility of Distributed Consensus with One Faulty Process**  
    FLP impossibility. Understand what distributed systems fundamentally cannot guarantee.
    
20. **Harvest, Yield, and Scalable Tolerant Systems**  
    Availability versus consistency tradeoffs.
    

### Tier 5: Modern infrastructure

21. **Dapper: a Large-Scale Distributed Systems Tracing Infrastructure**  
    Directly relevant to observability and debugging distributed systems.
    
22. **Borg: The Next Generation**  
    Useful after understanding the original Borg paper because it shows how large-scale scheduling evolved. [Google Research](https://research.google/pubs/borg-the-next-generation/?utm_source=chatgpt.com)
    
23. **Large-scale cluster management at Google with Borg** → **Kubernetes architecture/docs**  
    Don't just read papers here. Build things.
    
24. **Kafka: a Distributed Messaging System for Log Processing**  
    Learn distributed streaming architecture.
    
25. **Ray: A Distributed Framework for Emerging AI Applications**  
    This bridges your distributed-systems and AI interests.
    

### Tier 6: AI systems and infrastructure

Only **after** the above.

26. **Attention Is All You Need**
    
27. **LoRA: Low-Rank Adaptation of Large Language Models**
    
28. **QLoRA: Efficient Finetuning of Quantized LLMs**
    
29. **FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness**
    
30. **vLLM: Easy, Fast, and Cheap LLM Serving with PagedAttention**
    
31. **DistServe: Disaggregating Prefill and Decoding for Goodput-optimized Large Language Model Serving**
    
32. **Orca: A Distributed Serving System for Transformer-Based Generative Models**
    
33. **DeepSpeed: System Optimizations Enable Training Deep Learning Models with Over 100 Billion Parameters**
    
34. **Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism**
    
35. **LLM Serving in the Wild: An Empirical Study of Frameworks, Methods, and System Designs**  
    This is particularly useful now because it surveys how modern serving frameworks such as vLLM, SGLang, TensorRT-LLM and others are actually being used. [arXiv](https://arxiv.org/abs/2608.03036?utm_source=chatgpt.com)
    

### Tier 7: Your advanced territory

Once you're comfortable with the previous 35:

36. **SOSP/OSDI papers on distributed storage and compute**
    
37. **S-LoRA: Serving Thousands of Concurrent LoRA Adapters**
    
38. **Punica: Multi-Tenant LoRA Serving**
    
39. **DeltaServe: Host-Agnostic Co-Serving of Inference and Fine-Tuning for LLMs**  
    Very aligned with your LoRA + infrastructure interests. It explores using otherwise-idle inference capacity for fine-tuning while preserving inference SLOs. [arXiv](https://arxiv.org/abs/2607.28848?utm_source=chatgpt.com)
    
40. **Recent papers on LLM serving, distributed inference, KV-cache management, GPU scheduling and inference disaggregation**
    

---

### The path I'd specifically recommend for **you**

Don't randomly read hundreds of papers. Follow this progression:

**Distributed systems → databases → consensus → operating systems → networking → cloud infrastructure → observability → Kubernetes → distributed AI → LLM systems → AI infrastructure**

Your first **10** should therefore be:

> **GFS → MapReduce → Dynamo → Bigtable → Chubby → ZooKeeper → Paxos → Raft → Spanner → Borg**

That sequence will give you a much stronger systems foundation than jumping straight into LoRA, RAG, agents or LLM papers.

Google's current research catalogue still has hundreds of papers under distributed systems, networking and software systems, so there's plenty of depth to explore after this core sequence. [Google Research](https://research.google/pubs/?category=distributed-systems-and-parallel-computing&utm_source=chatgpt.com)