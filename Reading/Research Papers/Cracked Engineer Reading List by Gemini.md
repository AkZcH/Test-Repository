Here is the targeted topic-by-topic roadmap and reading list based on all 10 quiz questions.
## 1. AI Infrastructure & GPU Cluster Networking

**Quiz Topics Covered:**
- InfiniBand network topology, Adaptive Routing, and NCCL tuning ($NCCL\_ALGO$, $NCCL\_CROSS\_NIC$).
- Vector databases at billion-scale, approximate nearest neighbor search algorithms (HNSW), and quantization strategies (SQ8, Product Quantization).
    
**Core Reading List:**
- **Meta Llama 3 Infrastructure Paper:** Read the hardware and network optimization section of _The Llama 3 Herd of Models_ paper. It details real-world InfiniBand routing bottlenecks, packet drops, and fabric tuning across 16,000+ GPUs.

- **NVIDIA NCCL Developer Guide & Architecture Docs:** Study communication primitives ($AllReduce$, $AllGather$), Process Groups, and environment variables ($NCCL\_DEBUG$, $NCCL\_ALGO$, $NCCL\_P2P\_DISABLE$).

- **Pinecone & Qdrant Technical Blogs:** Read their deep dives on vector index algorithms, specifically _HNSW internals_, _Scalar Quantization (SQ8)_, and memory layout tradeoffs for billion-scale embeddings.
## 2. Kubernetes Internals & Advanced Diagnostics

**Quiz Topics Covered:**
- Pod termination lifecycle race conditions ($SIGTERM$, `preStop` hooks, and EndpointSlice controller propagation latency).
    
- Dynamic state drift, GitOps syncing mechanisms, and ArgoCD vs Argo Rollouts interaction ($ignoreDifferences$, $selfHeal$).
    
      
    

**Core Reading List:**

- **Kubernetes Docs & Blog:** _Graceful Shutdown and Zero Downtime in Kubernetes_ (explains how $kubelet$, EndpointSlices, and iptables interact asynchronously during pod deletion).
    
- **ArgoCD & Argo Rollouts Official Documentation:** Focus on _Architectural Concepts_, _Handling Drift_, _Sync Policies_, and _AnalysisTemplates with Prometheus integration_.
    
      
    

## 3. Systems Programming, Kernel Storage & Linux Internals

**Quiz Topics Covered:**

  

- Linux VFS (Virtual File System), unlinked open file descriptors, inode reservation, and zero-downtime log truncation using `/proc/<PID>/fd/`.
    
      
    

**Core Reading List:**

  

- **"Linux Kernel Development" by Robert Love:** Read the chapters on the Virtual File System (VFS), Inodes, Directory Entries (dentries), and File Descriptors.
    
      
    
- **Julia Evans' Sysadmin Zines & Blog:** Read her technical visual breakdowns on $lsof$, $du$ vs $df$ discrepancies, and `/proc` filesystem manipulation.
    
      
    

## 4. Low-Level Networking, eBPF & Hybrid Cloud BGP

**Quiz Topics Covered:**

  

- eBPF kernel socket filtering ($sockmap$, $sock\_ops$), short-circuiting TCP/IP stack traversals vs Envoy L7 sidecars.
    
      
    
- Hybrid cloud connectivity, BGP routing, hold-timer convergence limits, and Bidirectional Forwarding Detection (BFD).
    
      
    

**Core Reading List:**

  

- **Cilium Technical Documentation & Blog:** Read their architectural posts on _eBPF Host Routing_, _Kernel Bypassing with sockmap_, and _Istio Ambient Mesh vs Sidecars_.
    
      
    
- **"BGP in the Data Center" by Dinesh G. Dutt:** Focus on BGP timers, path selection algorithms, Equal-Cost Multi-Path (ECMP) routing, and sub-second fault detection via BFD.
    
      
    
- **AWS Direct Connect & Transit Gateway User Guides:** Study failover architectures between AWS Direct Connect (DX) and Site-to-Site IPsec VPNs using BGP and BFD.
    
      
    

## 5. Distributed Systems, Consensus & Cloud State

**Quiz Topics Covered:**

  

- Raft/Paxos consensus quorum mechanics ($N/2 + 1$) under network partitions vs 2-Phase Commit (2PC).
    
      
    
- Terraform/OpenTofu state locking, non-atomic resource creation failure recovery, and state reconciliation ($terraform\ import$).
    
      
    

**Core Reading List:**

  

- **"Designing Data-Intensive Applications" by Martin Kleppmann:** Chapters 8 and 9 (Distributed System Trouble, Consistency, and Consensus). This is the definitive text for understanding Raft, Paxos, and linearizability under CAP theorem constraints.
    
      
    
- **CockroachDB Docs - "Architecture Overview":** Read how Raft groups, range leases, and multi-region consensus function under real network split-brain scenarios.
    
      
    
- **HashiCorp Terraform Documentation:** Read _State Mechanism Internals_, _State Locking with DynamoDB_, and the _Import Workflow_.
    
      
    

## 6. Zero-Trust Security & Identity Federation

**Quiz Topics Covered:**

  

- AWS IAM OIDC identity federation with GitHub Actions, JWT claim verification ($sub$, $aud$), and runner scope token permissions.
    
      
    

**Core Reading List:**

  

- **AWS Security Blog:** Search for _About Security Processes for OpenID Connect in AWS IAM_.
    
      
    
- **GitHub Actions Security Documentation:** Read _Configuring OpenID Connect in Cloud Providers_ with a focus on token permissions ($permissions:\ id-token:\ write$) and JWT subject claim construction.


---
Here is the updated topic-by-topic roadmap with the exact direct links and documentation for everything we covered in the quiz.

  

### **1. AI Infrastructure & GPU Cluster Networking**

- **Meta Llama 3 Infrastructure Architecture:** Read the infrastructure and network optimization section of [The Llama 3 Herd of Models Paper](https://www.researchgate.net/publication/382739128_The_Llama_3_Herd_of_Models).
    
      
    
- **NVIDIA NCCL Developer Guide & Architecture Docs:** Study communication primitives, process groups, and tuning flags in the [NVIDIA NCCL Documentation](https://github.com/nvidia/nccl) and the [NVIDIA Deep Learning NCCL User Guide](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/index.html).
    
      
    
- **Vector Search Mechanics & Quantization:** Read the deep dive on [Hierarchical Navigable Small Worlds (HNSW) by Pinecone](https://www.pinecone.io/learn/series/faiss/hnsw/) and Qdrant’s guide on [Vector Quantization & SQ8 / Product Quantization](https://qdrant.tech/documentation/concepts/quantization/).
    
      
    

### **2. Kubernetes Internals & Progressive Delivery**

- **Pod Termination Lifecycle Race Conditions:** Learn how $kubelet$, EndpointSlices, and iptables interact during scale-down in [Graceful Shutdown and Zero Downtime in Kubernetes](https://medium.com/@mohit.agarwal_502/absolute-zero-downtime-with-graceful-shutdown-4eca0e9eb252) and [Kubernetes Pod Lifecycle Documentation](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/).
    
      
    
- **ArgoCD & Argo Rollouts Drift Mechanics:** Study how to prevent sync fights when dynamic deployment parameters shift via [How to Configure ignoreDifferences in ArgoCD](https://oneuptime.com/blog/post/2026-02-26-argocd-configure-ignore-differences/view) and the [Argo Rollouts Architectural Overview](https://argoproj.github.io/argo-rollouts/).
    
      
    

### **3. Linux Kernel Internals & Storage**

- **Linux Filesystem & Unlinked Open File Descriptors:** Understand how open inodes and `/proc` file descriptors lock block space using the [Red Hat Linux df and du Usage Guide](https://www.redhat.com/en/blog/linux-df-command) and Julia Evans' guide on [How to debug disk space issues with lsof and /proc](https://www.google.com/search?q=https://jvns.ca/blog/2016/11/20/proc-fd/).
    
      
    

### **4. Low-Level Networking, eBPF & Hybrid Cloud BGP**

- **eBPF Socket Acceleration & Envoy Comparison:** Read how kernel bypassing and $sockmap$ route traffic directly in the [Cilium eBPF Host Routing and Socket Layer Enforcement Docs](https://docs.cilium.io/en/stable/overview/intro/) and the [Cilium Engineering Blog](https://cilium.io/blog/).
    
      
    
- **Cloud BGP Failover & BFD Timers:** Study dynamic routing failover across hybrid links using [AWS Direct Connect BFD and Resiliency Architecture](https://docs.aws.amazon.com/directconnect/latest/UserGuide/resiliency_failover.html) and RFC 5880 on [Bidirectional Forwarding Detection (BFD)](https://datatracker.ietf.org/doc/html/rfc5880).
    
      
    

### **5. Distributed Systems, Consensus & Cloud State**

- **Consensus Quorum & Partition Tolerance:** Study Raft consensus mechanics under network partitions in the [CockroachDB Architecture Overview](https://www.google.com/search?q=https://www.cockroachlabs.com/docs/stable/architecture/overview.html) and Martin Kleppmann's [Designing Data-Intensive Applications (DDIA)](https://dataintensive.net/).
    
      
    
- **Terraform State Locks & Recovery:** Read about lock recovery and state reconciliation in the [Terraform State Internals & Import Guide](https://developer.hashicorp.com/terraform/cli/import).
    
      
    

### **6. Cloud Security & Identity Federation**

- **AWS IAM OIDC & GitHub Actions Federation:** Study token permission scopes ($permissions:\ id-token:\ write$) and claim evaluation in [Configuring OpenID Connect in AWS for GitHub Actions](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services).