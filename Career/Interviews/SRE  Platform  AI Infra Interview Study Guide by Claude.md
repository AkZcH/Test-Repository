[Chat](https://claude.ai/chat/0782dea9-673f-4ac5-b571-ba6f3ff0cc57)
Built from the 10 quiz questions. Each section has: what the question was really testing, core concepts to be able to explain from scratch, resources, a hands-on lab, and likely follow-up questions.

Note: URLs were written from memory of well-known, stable pages. If one has moved, search the page title.

---

## 0. General foundations (read these first)

- Google SRE Book and Workbook (free): https://sre.google/books/
- Brendan Gregg, Linux performance and the USE Method: https://www.brendangregg.com/linuxperf.html
- Kubernetes Failure Stories (real postmortems): https://k8s.af/
- _Designing Data-Intensive Applications_, Martin Kleppmann (book)
- _Systems Performance_, Brendan Gregg (book)

Interview habit worth building: for every incident-style question, answer in this order. Symptom, what you ruled out, the exact mechanism, the immediate fix, the permanent fix. The quiz questions all follow this shape.

---

## 1. GPU cluster networking and distributed training (Q1)

**Tests:** NCCL collectives, InfiniBand congestion, why one slow link stalls every rank.

**Core concepts**

- Data, tensor, pipeline parallelism and which collectives each uses (AllReduce, AllGather, ReduceScatter)
- Ring vs tree AllReduce, and why synchronous training makes the slowest link the step time
- NVLink (intra-node) vs InfiniBand or RoCE (inter-node), rail-optimized topologies
- ECMP hashing, flow collisions, adaptive routing, congestion control
- Reading NCCL logs (`NCCL_DEBUG=INFO`) and running `nccl-tests`

**Resources**

- NCCL user guide and env var reference: https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/
- NVIDIA `nccl-tests`: https://github.com/NVIDIA/nccl-tests
- Hugging Face Ultra-Scale Playbook: https://huggingface.co/spaces/nanotron/ultrascale-playbook
- Stas Bekman, _Machine Learning Engineering_ (debugging, networking, SLURM, failures): https://github.com/stas00/ml-engineering
- Megatron-LM scaling paper: https://arxiv.org/abs/2104.04473
- ZeRO paper: https://arxiv.org/abs/1910.02054
- Llama 3 paper, infrastructure and reliability sections: https://arxiv.org/abs/2407.21783

**Lab:** Rent two multi-GPU instances (or use a single node) and run `nccl-tests` `all_reduce_perf`. Compare `NCCL_ALGO=Ring` vs `Tree` and note bandwidth differences.

**Be ready to answer**

- Why does one congested link stall the whole job?
- When is Tree better than Ring?
- How would you detect a single bad node or link before it wastes a week of training?

**Honest caveat on the quiz:** option B bundles several settings (`NCCL_CROSS_NIC`, `NCCL_ALGO=Tree`) that are tuning knobs, not guaranteed fixes. In a real interview, lead with diagnosis (per-link counters, `nccl-tests` per node pair) and treat adaptive routing as the fabric-level fix.

---

## 2. Kubernetes graceful shutdown and 502s (Q2)

**Tests:** the pod termination race between SIGTERM and endpoint removal.

**Core concepts**

- Pod termination sequence: pod marked Terminating, endpoint removal and `preStop` hook start in parallel, then SIGTERM, then `terminationGracePeriodSeconds`, then SIGKILL
- EndpointSlice propagation to kube-proxy and to the ingress controller
- Readiness vs liveness probes, `preStop` sleep, app-level graceful shutdown (stop accepting, drain, exit)
- Nginx ingress behavior: upstream keepalive, retries, `proxy-next-upstream`

**Resources**

- Pod lifecycle and termination: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-termination
- Container lifecycle hooks: https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/
- EndpointSlices: https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/
- Learnk8s, graceful shutdown and zero downtime: https://learnk8s.io/graceful-shutdown
- ingress-nginx docs: https://kubernetes.github.io/ingress-nginx/

**Lab:** Deploy a small HTTP server behind ingress, hammer it with `hey` or `k6`, then roll the deployment with and without a `preStop: sleep`. Count the 502s.

**Be ready to answer**

- Why is `sleep 15` in `preStop` not a hack but a consequence of asynchronous control loops?
- What happens if `terminationGracePeriodSeconds` is shorter than the sleep plus drain time?
- What does `externalTrafficPolicy: Local` actually change? (It does not fix this problem.)

---

## 3. Terraform state, locking, and crash recovery (Q3)

**Tests:** how state is written, what locks protect, and import-based recovery.

**Core concepts**

- State is written after provider calls return, so a crash mid-apply can leave real resources missing from state
- Lock is separate from state; `force-unlock` only removes the lock
- `terraform import` and declarative `import` blocks (config-driven import)
- `refresh`, `plan -refresh-only`, `state list/show/rm/mv`
- S3 backend with versioning, DynamoDB locking (and newer native S3 locking), OpenTofu differences

**Resources**

- State overview: https://developer.hashicorp.com/terraform/language/state
- Import: https://developer.hashicorp.com/terraform/cli/import and https://developer.hashicorp.com/terraform/language/import
- force-unlock: https://developer.hashicorp.com/terraform/cli/commands/force-unlock
- S3 backend: https://developer.hashicorp.com/terraform/language/backend/s3
- OpenTofu docs: https://opentofu.org/docs/

**Lab:** Start an apply that creates a slow resource, `kill -9` it mid-way, force-unlock, and recover using `import`. Then redo recovery with an `import` block.

**Be ready to answer**

- Why does S3 versioning matter even though it was not the root cause here?
- How do you stop concurrent applies in CI? (Serialize pipelines per state, one state per blast radius.)
- When would you split a monolithic state file?

---

## 4. Linux disk space: `df` vs `du` and deleted-but-open files (Q4)

**Tests:** inodes, file descriptors, unlink semantics.

**Core concepts**

- A file's blocks are freed only when link count is 0 and no process holds it open
- `lsof +L1`, `lsof | grep deleted`, `/proc/<pid>/fd/`
- Truncating via `: > /proc/<pid>/fd/<n>` frees space without restarting the process
- Inode exhaustion (`df -i`) as the other classic cause
- Root-reserved blocks on ext4, overlay filesystems, container log rotation

**Resources**

- proc(5) man page: https://man7.org/linux/man-pages/man5/proc.5.html
- lsof man page: https://man7.org/linux/man-pages/man8/lsof.8.html
- Brendan Gregg Linux performance page (above)
- Kubernetes logging architecture and log rotation: https://kubernetes.io/docs/concepts/cluster-administration/logging/

**Lab:** Write a loop that logs to a file, `rm` the file, watch `df` stay full, find it with `lsof +L1`, free it via `/proc`.

**Be ready to answer**

- What is the difference between deleting a file and unlinking a name?
- What is the permanent fix? (Proper log rotation with `copytruncate` or reopen on SIGHUP, and size limits on container logs.)
- How do you tell block exhaustion from inode exhaustion in 10 seconds?

---

## 5. eBPF, sidecars, and ambient meshes (Q5)

**Tests:** where traffic interception happens and what each model costs.

**Core concepts**

- Sidecar path: iptables redirect into an Envoy in the pod, extra copies and context switches
- eBPF hooks: `sock_ops`, `sk_msg`, sockmap, tc/XDP, socket-level load balancing
- Istio ambient: per-node ztunnel (L4, mTLS) plus optional waypoint proxies (L7)
- Cilium service mesh: eBPF for L3/L4, Envoy per node for L7
- Why "eBPF replaces Envoy entirely" is wrong: L7 parsing still needs a proxy

**Resources**

- ebpf.io intro: https://ebpf.io/what-is-ebpf/
- Cilium docs: https://docs.cilium.io/
- Istio ambient overview: https://istio.io/latest/docs/ambient/
- _Learning eBPF_, Liz Rice (book, free from Isovalent)
- Brendan Gregg, _BPF Performance Tools_ (book)

**Lab:** Install Cilium on a kind cluster, enable Hubble, and observe flows. Then install Istio in sidecar mode and ambient mode and compare per-pod resource use.

**Be ready to answer**

- What can an eBPF program see at the socket layer vs at tc?
- Where does mTLS terminate in ambient mode?
- What are the downsides of ambient or sidecarless (shared proxy blast radius, kernel version requirements)?

---

## 6. GitOps, ArgoCD, and Argo Rollouts (Q6)

**Tests:** two controllers fighting over one object.

**Core concepts**

- Desired state in Git vs live state, sync status, `selfHeal`, `prune`, auto-sync
- `ignoreDifferences` and `RespectIgnoreDifferences` sync option
- Argo Rollouts strategies, `AnalysisTemplate`, abort vs rollback semantics
- ApplicationSets and fleet management across many clusters
- Image Updater and write-back to Git

**Resources**

- ArgoCD diffing customization: https://argo-cd.readthedocs.io/en/stable/user-guide/diffing/
- Auto-sync and self-heal: https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/
- ApplicationSet: https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/
- Argo Rollouts: https://argoproj.github.io/argo-rollouts/
- OpenGitOps principles: https://opengitops.dev/

**Lab:** Deploy a Rollout via ArgoCD with a failing AnalysisTemplate. Observe the abort and what ArgoCD reports. Then add `ignoreDifferences` for the fields Rollouts mutates.

**Be ready to answer**

- Why is disabling `selfHeal` globally a bad answer?
- Who owns `replicas` when an HPA is also in play?
- Honest note: in real Argo Rollouts, an abort does not rewrite the spec in the cluster, so the quiz scenario is somewhat stylized. Know the real abort behavior and say so if challenged.

---

## 7. Consensus, Raft/Paxos, and linearizability (Q7)

**Tests:** quorum math and the CAP trade-off.

**Core concepts**

- Raft: leader election, log replication, commit index, terms, leases
- Quorum is a majority (2 of 3, 3 of 5), not unanimity
- Why 2PC is a different problem (cross-shard atomicity), and how it layers on top of consensus
- Leaseholders, read leases, clock skew, follower reads
- Partition behavior: the majority side stays available, the minority side cannot commit

**Resources**

- Raft paper: https://raft.github.io/raft.pdf
- Raft visualization: https://thesecretlivesofdata.com/raft/
- MIT 6.5840 (6.824) labs and lectures: https://pdos.csail.mit.edu/6.824/
- Jepsen consistency map: https://jepsen.io/consistency
- CockroachDB architecture overview: https://www.cockroachlabs.com/docs/stable/architecture/overview
- _Designing Data-Intensive Applications_, chapters 5, 7, 9

**Lab:** Implement Raft leader election and log replication (MIT 6.5840 Lab 2 is the gold standard). This also fits your Rust WAL project direction.

**Be ready to answer**

- In the quiz partition (East vs EU), can the East leader still commit? (Yes, if it can reach West for a quorum of 2.)
- What breaks linearizability even with Raft? (Stale leader reads without leases or read-index.)
- Difference between Raft replication and 2PC.

---

## 8. GitHub Actions OIDC to AWS (Q8)

**Tests:** JWT claims, trust policy conditions, and workflow permissions.

**Core concepts**

- OIDC flow: workflow requests a JWT, calls `AssumeRoleWithWebIdentity`, AWS validates issuer, `aud`, and `sub`
- `permissions: id-token: write` is required in the workflow
- `sub` claim formats change with context: branch ref, `pull_request`, `environment:<name>`, tags, reusable workflows
- `StringEquals` vs `StringLike` in trust policies, and the danger of overly wide wildcards
- Customizing `sub` claims at the org or repo level

**Resources**

- GitHub, OIDC with AWS: https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services
- GitHub, OIDC reference and claims: https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect
- `aws-actions/configure-aws-credentials`: https://github.com/aws-actions/configure-aws-credentials
- AWS IAM OIDC providers: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html

**Lab:** Set up OIDC for a test repo. Deliberately break it three ways (missing `id-token: write`, wrong `sub`, job using an `environment`) and compare the error messages.

**Be ready to answer**

- How would you debug a `sub` mismatch? (Print the decoded JWT claims in a throwaway job.)
- Honest note: a `sub` mismatch is the more common cause of the exact error text in the quiz, especially when a job runs under an `environment`. A missing `id-token: write` normally fails earlier, at token request time. If you hit this on a test, pick the answer the quiz wants, but know the real-world distinction.

---

## 9. Vector databases at billion scale (Q9)

**Tests:** index structures and quantization trade-offs.

**Core concepts**

- Memory math: N x dims x bytes. 1B x 1536 x 4 bytes is about 6 TB in FP32
- HNSW, IVF, IVF-PQ, DiskANN/Vamana, and their memory and recall profiles
- Scalar (SQ8), product (PQ), and binary quantization; rescoring with original vectors
- Matryoshka embeddings and dimension truncation
- Sharding, replication, filtered search, p99 latency under load
- Note: `text-embedding-3-large` is natively 3072 dims, 1536 is `-small`. The quiz mixes these.

**Resources**

- HNSW paper: https://arxiv.org/abs/1603.09320
- FAISS wiki (index types, PQ, guidelines for choosing an index): https://github.com/facebookresearch/faiss/wiki
- Qdrant quantization guide: https://qdrant.tech/documentation/guides/quantization/
- Milvus docs (index types): https://milvus.io/docs
- DiskANN paper: https://proceedings.neurips.cc/paper/2019/hash/09853c7fb1d3f8ee67a61b6bf4a7f8e6-Abstract.html
- ANN-Benchmarks: http://ann-benchmarks.com/

**Lab:** Take a public dataset (SIFT1M or a sample of embeddings), build FAISS `IndexHNSWFlat`, `IndexIVFPQ`, and an SQ8 variant. Plot memory vs recall@10 vs latency. This ties directly into your Cairn RAG work.

**Be ready to answer**

- SQ8 gives about 4x, PQ can give 16x to 64x or more. Why does PQ cost more recall?
- What is rescoring and why does it recover recall?
- When do you choose disk-based indexes over RAM-resident HNSW?

---

## 10. Hybrid networking: Direct Connect, BGP, BFD (Q10)

**Tests:** failure detection time vs routing convergence.

**Core concepts**

- BGP keepalive and hold timers (defaults are slow), why a physical fiber cut may not be detected quickly
- BFD: sub-second liveness detection that tells BGP to tear down fast
- AWS DX with DXGW, Transit VIF, VPN backup, route preference (local pref, AS-path prepending, longest prefix match)
- Asymmetric routing and which side controls which direction (you control egress preference with local pref, AWS side sees your advertisements)
- Direct Connect resiliency models (multiple locations, multiple connections)

**Resources**

- AWS Direct Connect docs: https://docs.aws.amazon.com/directconnect/
- AWS Direct Connect Resiliency Recommendations: search "AWS Direct Connect Resiliency Toolkit"
- BFD RFC 5880: https://datatracker.ietf.org/doc/html/rfc5880
- BGP RFC 4271: https://datatracker.ietf.org/doc/html/rfc4271
- AWS Whitepaper, "Building a scalable and secure multi-VPC AWS network infrastructure": search the title on docs.aws.amazon.com
- _BGP_ by Iljitsch van Beijnum (book, short and practical)

**Lab:** Use FRRouting in two containers or VMs, configure a BGP session, enable BFD, and kill the link. Compare detection time with and without BFD.

**Be ready to answer**

- Why is BFD needed when BGP already has keepalives?
- How do you make VPN the backup path rather than the primary? (Prefer DX via local preference, longer AS-path on VPN advertisements.)
- What does graceful restart do, and why can it hurt fast failover?

---

## Suggested 3-week plan

|Week|Focus|Output|
|---|---|---|
|1|Kubernetes shutdown race, Linux `df`/`du`, Terraform state|Three short write-ups, each with a reproduced failure|
|2|Raft, GitOps, OIDC, BGP/BFD|Raft lab progress, an OIDC demo repo, a FRR lab|
|3|GPU networking, eBPF/mesh, vector scaling|`nccl-tests` results, Cilium vs Istio comparison, FAISS benchmark chart|

## How to use this for interviews

1. For each topic, write a 60-second explanation without notes, then a 5-minute whiteboard version.
2. Keep a one-page "incident card" per section: symptom, mechanism, fix, prevention.
3. Reproduce at least one failure per topic yourself. Interviewers can tell the difference between recalled and lived experience.
4. Be ready to critique the "obvious" answer. Several quiz options are plausible but wrong for specific reasons, and explaining why is where strong candidates separate themselves.