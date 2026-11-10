---

title: "Computer Systems, HPC & Infrastructure Roadmap"

aliases:

  - Systems Engineering Roadmap

  - Computer Architecture and HPC Roadmap

tags:

  - computer-science

  - computer-systems

  - computer-architecture

  - operating-systems

  - linux

  - hpc

  - numa

  - distributed-systems

  - ai-infrastructure

status: active

created: 2026-11-08

estimated_duration: 18-24 months

---

  

# Computer Systems, HPC & Infrastructure Roadmap

  

> [!abstract] The objective

> Understand how a modern computer works from the CPU and memory hierarchy up to operating systems, multi-socket servers, supercomputers, datacenters, and AI infrastructure. The goal is to become capable of explaining, measuring, debugging, and improving real systems, not merely knowing terminology.

  

## 1. What this roadmap covers

  

This roadmap connects several related fields:

  

- **Computer architecture:** how processors, caches, memory, and I/O are designed.

- **Operating systems:** how software manages hardware, processes, memory, files, and devices.

- **Systems programming:** how to write software that interacts closely with the operating system and hardware.

- **Performance engineering:** how to find and remove bottlenecks using measurement.

- **NUMA and multiprocessor systems:** how CPUs, memory, and devices are arranged inside large servers.

- **Parallel computing and HPC:** how to divide computational work across cores, processors, GPUs, and machines.

- **Datacenter architecture:** how servers, networks, storage, power, and cooling fit together.

- **Distributed systems:** how independent machines coordinate, replicate data, and tolerate failures.

- **AI infrastructure:** how GPUs, networking, memory, and serving systems support large AI workloads.

  

These fields overlap, but they are not interchangeable. NUMA is primarily about locality inside a multi-socket machine. MPI is a programming model for parallel processes that can span machines. Distributed systems focuses on coordination and correctness across machines that communicate over a network.

  

## 2. Time commitment and operating rules

  

**Target duration:** 18-24 months at approximately 8-10 focused hours per week. This is an estimate, not a deadline. A slower pace is better than rushing through material without being able to use it.

  

Suggested weekly allocation:

  

- 3 hours: course lectures or textbook reading.

- 4-5 hours: coding, labs, experiments, and debugging.

- 1 hour: notes, diagrams, and explaining concepts in your own words.

- 1 hour: reviewing benchmarks, documenting results, and planning the next week.

  

### Rules

  

1. **Build while learning.** Every major phase must produce working code, an experiment, or a written technical explanation.

2. **Measure before optimizing.** Record a baseline before changing code or configuration.

3. **Keep experiments reproducible.** Record hardware, OS, compiler, compiler flags, workload size, and command used.

4. **Do not collect resources endlessly.** Choose one primary course or book per phase. Use the other links as references.

5. **Do not buy a server yet.** A normal Linux machine is enough for most of the roadmap. Use remote systems or cloud/HPC resources later when physical multi-socket hardware is required.

6. **Do not confuse tool familiarity with understanding.** Being able to run `perf`, `numactl`, or `mpirun` is not the same as understanding the result.

7. **Write a short engineering report for every substantial project.** Include the question, setup, method, results, limitations, and what you learned.

  

## 3. Environment and toolkit

  

### Recommended environment

  

Use a Linux environment for the systems work. Native Linux or a dedicated Linux machine is preferable for low-level performance experiments. WSL2 is fine for early C, shell, and OS exercises, but virtualization can obscure or constrain hardware topology, performance counters, and NUMA experiments. Do not treat WSL2 measurements as bare-metal hardware results.

  

Recommended setup:

  

- Ubuntu LTS or another mainstream Linux distribution.

- GCC and Clang.

- Make and CMake.

- Git.

- GDB.

- Valgrind.

- `strace` and `ltrace`.

- `perf`.

- `htop`, `procps`, `sysstat`, and `linux-tools`.

- `numactl` and `hwloc` for later phases.

- Python for plotting benchmark output and analysing results.

  

Useful references:

  

- [Linux manual pages](https://man7.org/linux/man-pages/)

- [GCC documentation](https://gcc.gnu.org/onlinedocs/)

- [Clang documentation](https://clang.llvm.org/docs/)

- [GDB documentation](https://sourceware.org/gdb/documentation/)

- [Valgrind documentation](https://valgrind.org/docs/manual/)

- [Compiler Explorer](https://godbolt.org/)

- [CMake documentation](https://cmake.org/documentation/)

  

### Initial setup checklist

  

- [ ] Install or provision a Linux environment.

- [ ] Set up Git and a dedicated repository for experiments.

- [ ] Install GCC, Clang, GDB, Make, CMake, and Python.

- [ ] Learn to compile with warnings enabled: `-Wall -Wextra -Wpedantic`.

- [ ] Learn to build debug and optimized binaries: `-O0 -g` and `-O2`.

- [ ] Create a benchmark-results directory and store raw output as CSV or JSON.

- [ ] Record the machine's basic information using `uname -a`, `lscpu`, `free -h`, and `lsblk`.

  

---

  

# Part I: Understand the machine

  

## Phase 0 — Digital logic and computer fundamentals

  

**Estimated duration:** 2-3 weeks  

**Priority:** Foundational

  

### Learn

  

- Binary and hexadecimal representation.

- Bits, bytes, words, and address spaces.

- Boolean logic and logic gates.

- Signed and unsigned integers.

- Two's complement.

- Floating-point representation and IEEE 754 basics.

- Registers, instructions, and the fetch-decode-execute cycle.

- The difference between an ISA and a CPU microarchitecture.

  

### Primary resource

  

- [Nand2Tetris](https://www.nand2tetris.org/)

  - Work through selected Part I material, especially Boolean logic, arithmetic, memory, and computer architecture.

  - The course has 12 projects in total. You do not need to finish every project before moving on.

  

### Supporting resources

  

- [Nand2Tetris project list](https://www.nand2tetris.org/course)

- [CS:APP official site](https://csapp.cs.cmu.edu/3e/perspective.html)

  

### Practical work

  

- [ ] Write a binary/hexadecimal converter.

- [ ] Write a small program demonstrating signed integer overflow and integer representation. Avoid relying on undefined C behavior when testing it.

- [ ] Convert IEEE 754 bit patterns to readable floating-point values.

- [ ] Draw the basic path from a C statement to machine instructions.

  

### Completion criteria

  

You can explain what a bit, register, instruction, memory address, ISA, and CPU core are without relying on vague analogies.

  

---

  

## Phase 1 — C and systems programming foundations

  

**Estimated duration:** 6-8 weeks  

**Priority:** Essential

  

C is the main language for this roadmap because it exposes memory, pointers, data layout, compilation, and operating-system interfaces directly. You can use Rust or Go for later projects, but do not avoid C if the objective is to understand low-level systems.

  

### Learn

  

**C language and memory**

  

- Variables, types, scope, and lifetime.

- Pointers and pointer arithmetic.

- Arrays, strings, structs, unions, and enums.

- Stack and heap allocation.

- `malloc`, `calloc`, `realloc`, and `free`.

- Function pointers and callbacks.

- Alignment and structure padding.

- Undefined behavior and common memory bugs.

  

**Build and execution**

  

- Preprocessing, compilation, assembly, and linking.

- Object files and executable formats.

- Static and dynamic libraries.

- Debug symbols.

- System calls versus library calls.

- Process memory layout.

  

**Assembly basics**

  

- General-purpose registers.

- Stack pointer and instruction pointer.

- Load/store operations.

- Function calling conventions.

- Stack frames.

- Conditional branches and loops.

- Basic x86-64 assembly reading.

  

### Primary resources

  

- [Computer Systems: A Programmer's Perspective (CS:APP)](https://csapp.cs.cmu.edu/3e/perspective.html)

- [AMD64 Architecture Programmer's Manual](https://docs.amd.com/api/khub/documents/68GKiN0gMEd6bMddsmhPwg/content)

- [Compiler Explorer](https://godbolt.org/)

  

### Supporting tools

  

- `gcc -S` to generate assembly.

- `objdump -d` to disassemble binaries.

- `readelf -a` to inspect ELF files.

- `gdb` to inspect program execution.

- `valgrind` to detect memory errors and leaks.

  

### Projects

  

- [ ] Implement a dynamic array in C.

- [ ] Implement a linked list, stack, and hash table.

- [ ] Build a basic arena allocator.

- [ ] Write a program that intentionally leaks memory, then find and fix the leak.

- [ ] Inspect the assembly generated for loops, function calls, and arithmetic operations.

- [ ] Inspect an ELF executable with `readelf` and `objdump`.

  

### Completion criteria

  

You can trace a small C program through compilation, inspect its assembly, explain its memory layout, and debug a segmentation fault with GDB.

  

---

  

## Phase 2 — Computer architecture

  

**Estimated duration:** 6-8 weeks  

**Priority:** Essential

  

This phase explains why processors behave the way they do. It is central to understanding the EPYC comparison that started this roadmap.

  

### Learn

  

**CPU design**

  

- Instruction Set Architecture (ISA).

- Microarchitecture.

- CPU pipelines.

- Instruction fetch, decode, and execution.

- Superscalar execution.

- Out-of-order execution.

- Register renaming.

- Branch prediction and speculative execution.

- Simultaneous multithreading (SMT).

- SIMD and vector instructions.

  

**Memory hierarchy**

  

- Registers, L1, L2, and L3 cache.

- Cache lines and cache associativity.

- Cache hits and misses.

- Temporal and spatial locality.

- Hardware prefetching.

- Translation Lookaside Buffer (TLB).

- Virtual memory and page tables.

- DRAM latency and bandwidth.

  

**Multiprocessor concepts**

  

- Cores versus sockets.

- Shared and private caches.

- Cache coherence.

- Cache-coherence protocols at a conceptual level.

- Memory consistency models.

- CPU interconnects.

- PCIe and I/O basics.

  

### Primary resource

  

- [MIT 6.5900 — Computer System Architecture](https://csg.csail.mit.edu/6.5900/info.html)

  

### Supporting resources

  

- [CS:APP](https://csapp.cs.cmu.edu/3e/perspective.html)

- [AMD64 Architecture Programmer's Manual](https://docs.amd.com/api/khub/documents/68GKiN0gMEd6bMddsmhPwg/content)

- [Nand2Tetris](https://www.nand2tetris.org/)

  

### Projects and experiments

  

- [ ] Measure sequential versus random memory access.

- [ ] Write a program that walks an array with different strides.

- [ ] Measure how working-set size affects performance as it crosses cache levels.

- [ ] Compare a predictable branch with a difficult-to-predict branch.

- [ ] Compare a data layout based on array-of-structures with structure-of-arrays.

- [ ] Inspect compiler vectorization reports.

- [ ] Use `perf stat` to collect cycles, instructions, cache-related events, and branch events where supported.

  

### Completion criteria

  

You can explain why two programs doing similar amounts of arithmetic can have very different runtimes because of memory access patterns, cache behavior, branching, or instruction-level parallelism.

  

---

  

# Part II: Understand the operating system

  

## Phase 3 — Operating systems

  

**Estimated duration:** 8-10 weeks  

**Priority:** Essential

  

### Learn

  

**Processes and execution**

  

- Processes and threads.

- Process creation and termination.

- Context switching.

- CPU scheduling.

- System calls.

- User mode and kernel mode.

- Inter-process communication.

  

**Memory management**

  

- Virtual address spaces.

- Paging and page tables.

- Page faults.

- TLBs.

- Copy-on-write.

- Memory-mapped files.

- Swapping and page reclaim.

  

**Concurrency**

  

- Threads and synchronization.

- Mutexes, semaphores, and condition variables.

- Deadlocks and race conditions.

  

**Persistence and I/O**

  

- Filesystems and inodes.

- File descriptors.

- I/O devices and interrupts.

- Buffering and caching.

- Basic storage stack.

  

### Primary resources

  

- [Operating Systems: Three Easy Pieces (OSTEP)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

- [MIT 6.1810 — Operating System Engineering](https://pdos.csail.mit.edu/6.828/2025/overview.html)

  

### Practical course work

  

- Read OSTEP by topic and complete its relevant exercises.

- Complete selected MIT xv6 labs. Do not merely watch the lectures.

- [MIT xv6 source and book](https://pdos.csail.mit.edu/6.1810/2025/xv6.html)

  

### Projects

  

- [ ] Write a small Unix shell that supports command execution, arguments, and basic pipes.

- [ ] Write a program using `fork`, `exec`, and `wait`.

- [ ] Write a program using `mmap` and inspect its memory mappings.

- [ ] Implement a producer-consumer program using pthreads and condition variables.

- [ ] Complete several xv6 labs involving system calls, virtual memory, or scheduling.

  

### Completion criteria

  

You can explain what happens when a program is launched, how it gets memory, how the kernel schedules it, and how it interacts with files and devices.

  

---

  

## Phase 4 — Linux internals and practical administration

  

**Estimated duration:** 6-8 weeks  

**Priority:** Essential for systems and infrastructure work

  

The objective is to connect operating-system concepts to the Linux system you actually use.

  

### Learn

  

- Linux process model and `/proc`.

- Process states and signals.

- Scheduler basics.

- Virtual memory and page cache.

- Filesystems and block I/O.

- Kernel versus user space.

- System calls and tracing.

- cgroups and namespaces at a conceptual level.

- Kernel logs and troubleshooting.

- CPU, memory, disk, and network observability.

  

### Tools to master

  

```bash

ps

top

htop

vmstat

mpstat

pidstat

iostat

sar

free

lsof

strace

ltrace

journalctl

dmesg

lsblk

lspci

lscpu

```

  

### Primary resources

  

- [Linux kernel documentation](https://www.kernel.org/doc/html/latest/)

- [Linux man-pages project](https://man7.org/linux/man-pages/)

- [Brendan Gregg — Linux Performance](https://www.brendangregg.com/linuxperf.html)

  

### Projects

  

- [ ] Build a process-inspection script that reports CPU and memory usage.

- [ ] Trace a program's system calls with `strace` and explain the important calls.

- [ ] Investigate a deliberately CPU-bound process, memory-heavy process, and I/O-heavy process.

- [ ] Inspect `/proc/<pid>/status`, `/proc/<pid>/maps`, and `/proc/<pid>/smaps`.

- [ ] Create a short Linux troubleshooting runbook for CPU, memory, disk, and process issues.

  

### Completion criteria

  

Given a slow or misbehaving Linux process, you can collect useful evidence before changing configuration or blaming the application.

  

---

  

# Part III: Concurrency, performance, and NUMA

  

## Phase 5 — Concurrency and parallel programming fundamentals

  

**Estimated duration:** 6-8 weeks  

**Priority:** Essential before serious NUMA or HPC work

  

### Learn

  

- Concurrency versus parallelism.

- Threads and thread pools.

- Mutexes and read-write locks.

- Semaphores and condition variables.

- Atomic operations and compare-and-swap.

- Memory ordering: relaxed, acquire, and release.

- Data races and race detection.

- Deadlocks, livelocks, and starvation.

- False sharing.

- Work queues and work stealing.

- Scalability and synchronization overhead.

  

### Resources

  

- [OSTEP — Concurrency](https://pages.cs.wisc.edu/~remzi/OSTEP/)

- [Linux kernel memory barriers](https://www.kernel.org/doc/html/latest/core-api/wrappers/memory-barriers.html)

- [Linux kernel atomic operations](https://www.kernel.org/doc/html/latest/core-api/wrappers/atomic_t.html)

- [CS:APP](https://csapp.cs.cmu.edu/3e/perspective.html)

  

### Projects

  

- [ ] Build a fixed-size thread pool.

- [ ] Build a producer-consumer work queue.

- [ ] Implement a concurrent counter with a mutex and with atomics.

- [ ] Implement a bounded concurrent queue.

- [ ] Benchmark throughput as thread count increases.

- [ ] Demonstrate false sharing, then fix it using appropriate data layout and alignment.

- [ ] Use a race detector where supported, such as ThreadSanitizer.

  

### Completion criteria

  

You can explain why adding threads can make a program slower, and can identify contention, synchronization overhead, and false sharing in a benchmark.

  

---

  

## Phase 6 — Performance engineering and benchmarking

  

**Estimated duration:** 4-6 weeks, then ongoing  

**Priority:** Essential

  

Performance engineering is not guessing which language or machine is faster. It is forming a hypothesis, measuring the system, identifying a bottleneck, changing one thing, and measuring again.

  

### Learn

  

- Latency, throughput, and bandwidth.

- CPU-bound, memory-bound, and I/O-bound workloads.

- CPU utilization versus CPU saturation.

- IPC and CPI.

- Cache and branch misses.

- Profiling versus benchmarking.

- Warm-up, repetitions, variance, and confidence.

- Amdahl's Law and limits to parallel speedup.

- Roofline model and arithmetic intensity.

- Flame graphs.

- The USE method: utilization, saturation, and errors.

  

### Primary resources

  

- [Systems Performance, 2nd Edition — Brendan Gregg](https://www.brendangregg.com/systems-performance-2nd-edition-book.html)

- [Brendan Gregg — Linux Performance](https://www.brendangregg.com/linuxperf.html)

- [Linux kernel — workload tracing and perf](https://www.kernel.org/doc/html/latest/admin-guide/workload-tracing.html)

- [Berkeley Lab — Roofline Model](https://amcr.lbl.gov/departments/computer-science-department/ppan/roofline-performance-model/)

  

### Tools

  

```bash

perf stat

perf record

perf report

perf top

strace -c

vmstat

iostat

pidstat

```

  

Also explore:

  

- [FlameGraph](https://github.com/brendangregg/FlameGraph)

- [bpftrace](https://bpftrace.org/)

- [BCC](https://github.com/iovisor/bcc)

  

### Project: Performance investigation report

  

Choose a program you have already built.

  

- [ ] Establish a reproducible baseline.

- [ ] Measure runtime and throughput.

- [ ] Profile CPU usage.

- [ ] Determine whether the workload is CPU-, memory-, or I/O-bound.

- [ ] Identify one bottleneck supported by evidence.

- [ ] Make one targeted optimization.

- [ ] Repeat the benchmark under the same conditions.

- [ ] Explain the improvement, trade-offs, and remaining bottleneck.

  

### Completion criteria

  

You can support a performance claim with measurements and explain why the result changed. Do not set an arbitrary target such as "10x faster" before understanding the workload.

  

---

  

## Phase 7 — NUMA and multi-socket systems

  

**Estimated duration:** 4-6 weeks  

**Priority:** Core to the original EPYC interest

  

This phase is the reason to study the earlier material. NUMA is much easier to understand after you know virtual memory, CPU caches, concurrency, scheduling, and performance measurement.

  

### Learn

  

- What NUMA means and why it exists.

- NUMA nodes and CPU-to-memory locality.

- Local versus remote memory access.

- Memory latency versus memory bandwidth.

- First-touch allocation policy.

- CPU affinity and memory affinity.

- NUMA-aware scheduling and allocation.

- Inter-socket links and remote access costs.

- Page migration and automatic NUMA balancing.

- NUMA effects on databases, virtual machines, and parallel workloads.

- PCIe device locality and I/O affinity.

  

### Resources

  

- [Linux `numa(7)` manual page](https://linuxman7.org/linux/man-pages/man7/numa.7.html)

- [hwloc — Hardware Locality](https://www.open-mpi.org/projects/hwloc/)

- [hwloc documentation](https://hwloc.readthedocs.io/)

- [AMD EPYC 9004 Series](https://www.amd.com/en/products/processors/server/epyc/4th-generation-9004-and-8004-series.html)

- [AMD EPYC 9005 Series](https://www.amd.com/en/products/processors/server/epyc/9005-series.html)

- [Linux kernel documentation](https://www.kernel.org/doc/html/latest/)

  

### Tools

  

```bash

lscpu

numactl --hardware

numastat

numactl

taskset

lstopo

```

  

Useful files to inspect:

  

```text

/sys/devices/system/node/

/proc/<pid>/numa_maps

/proc/<pid>/status

```

  

### Projects

  

- [ ] Inspect the CPU and NUMA topology of a Linux machine using `lscpu` and `lstopo`.

- [ ] Run a memory benchmark with CPU and memory bound to the same NUMA node.

- [ ] Repeat with CPU and memory bound to different nodes, if the hardware exposes multiple nodes.

- [ ] Compare local and remote memory bandwidth and latency.

- [ ] Compare default allocation with explicit NUMA policies using `numactl`.

- [ ] Pin threads to specific CPUs and measure the impact.

- [ ] Write a report explaining the topology and the limitations of the experiment.

- [ ] As a paper exercise, compare a single-socket EPYC 9965 platform with a dual-socket 2 × EPYC 9654 platform. Separate core count, memory channels, aggregate bandwidth, cache capacity, PCIe connectivity, power, and NUMA locality. Mark theoretical specifications separately from measured workload performance.

  

### Hardware reality check

  

A typical laptop may expose one NUMA node. A virtual machine may hide the host's physical topology. You can learn NUMA concepts on any machine, but a convincing local-versus-remote experiment requires a system that exposes multiple NUMA nodes. Use a university lab, a borrowed server, a suitable bare-metal cloud instance, or a remote HPC system. Do not buy a dual-socket EPYC server just to begin learning.

  

### Completion criteria

  

You can explain why remote memory access exists, inspect a system's NUMA topology, pin a process, control memory placement, and interpret the resulting measurements.

  

---

  

# Part IV: Parallel computing and HPC

  

## Phase 8 — Shared-memory parallelism with OpenMP

  

**Estimated duration:** 4-6 weeks  

**Priority:** Essential for HPC

  

### Learn

  

- Data parallelism and task parallelism.

- Parallel loops.

- OpenMP thread teams.

- Work-sharing constructs.

- Reductions.

- Synchronization and barriers.

- Scheduling policies.

- Thread affinity.

- Nested parallelism.

- False sharing and memory bandwidth saturation.

  

### Resources

  

- [OpenMP specifications](https://www.openmp.org/specifications/)

- [OpenMP tutorials and articles](https://www.openmp.org/resources/tutorials-articles/)

- [NERSC OpenMP documentation](https://docs.nersc.gov/development/openmp/)

  

### Projects

  

- [ ] Parallelize a vector addition program.

- [ ] Implement matrix multiplication sequentially and with OpenMP.

- [ ] Implement a parallel reduction.

- [ ] Implement parallel sorting or image processing.

- [ ] Measure speedup at 1, 2, 4, 8, and more threads where available.

- [ ] Identify when performance stops scaling and explain why.

  

### Completion criteria

  

You can write a shared-memory parallel program, measure scaling, and explain overheads and bottlenecks.

  

---

  

## Phase 9 — Distributed-memory parallelism with MPI

  

**Estimated duration:** 4-6 weeks  

**Priority:** Essential for traditional HPC

  

OpenMP primarily expresses parallelism within a shared-memory machine. MPI expresses communication between separate processes. Those processes can be placed on different nodes in a cluster.

  

### Learn

  

- Processes and ranks.

- Point-to-point communication.

- Blocking and non-blocking communication.

- Broadcast, scatter, gather, and reduce.

- Communicators.

- Collective operations.

- Message size and communication overhead.

- Strong scaling and weak scaling.

- Hybrid MPI + OpenMP programs.

  

### Resources

  

- [MPI Tutorial](https://mpitutorial.com/tutorials/)

- [Open MPI documentation](https://www.open-mpi.org/doc/)

- [NERSC programming models](https://docs.nersc.gov/development/programming-models/)

  

### Projects

  

- [ ] Write an MPI Hello World program and understand ranks.

- [ ] Implement parallel numerical integration.

- [ ] Implement a distributed matrix multiplication.

- [ ] Use broadcast, scatter, gather, and reduce.

- [ ] Measure communication overhead as message size changes.

- [ ] Run a hybrid MPI + OpenMP program if you have access to a multi-core environment.

  

### Completion criteria

  

You can explain the difference between shared-memory and distributed-memory parallelism, and identify when communication overhead limits scaling.

  

---

  

## Phase 10 — HPC systems and cluster operations

  

**Estimated duration:** 4-6 weeks  

**Priority:** Strongly recommended

  

### Learn

  

- Compute nodes and login nodes.

- CPU and accelerator partitions.

- Interconnects and cluster fabrics.

- Parallel filesystems.

- Batch job scheduling.

- Job queues and resource requests.

- Job arrays.

- Checkpointing.

- Resource utilization.

- Cluster monitoring and fair-share scheduling.

  

### Resources

  

- [NERSC documentation](https://docs.nersc.gov/)

- [NERSC programming models](https://docs.nersc.gov/development/programming-models/)

- [Slurm documentation](https://slurm.schedmd.com/documentation.html)

  

### Practical work

  

- [ ] Create and run a batch job using Slurm, if you have access to a Slurm cluster.

- [ ] Request CPU cores and memory explicitly.

- [ ] Inspect job state, resource usage, and exit status.

- [ ] Run a small MPI workload through the scheduler.

- [ ] Compare interactive execution with batch execution.

- [ ] Document the lifecycle of a submitted job.

  

### Completion criteria

  

You understand how an HPC cluster allocates shared resources and can submit, monitor, and debug a basic parallel job.

  

---

  

# Part V: Accelerators and large-scale infrastructure

  

## Phase 11 — GPU architecture and GPU programming

  

**Estimated duration:** 6-8 weeks  

**Priority:** Optional for general systems engineering; important for AI infrastructure and GPU-heavy HPC

  

Do not assume a GPU is simply a faster CPU. GPUs are designed around massive parallelism, throughput, and a different memory hierarchy.

  

### Learn

  

- CPU versus GPU execution models.

- GPU compute units and streaming multiprocessors.

- Threads, warps, blocks, and grids.

- Global, shared, and local memory.

- Memory coalescing.

- Occupancy and register pressure.

- Host-to-device transfers.

- Kernel launch overhead.

- GPU profiling and utilization.

- Multi-GPU communication at a conceptual level.

  

### Resources

  

- [NVIDIA CUDA Programming Guide](https://docs.nvidia.com/cuda/cuda-programming-guide/index.html)

- [NVIDIA CUDA Samples](https://github.com/NVIDIA/cuda-samples)

- [NVIDIA NCCL documentation](https://docs.nvidia.com/deeplearning/nccl/)

  

### Projects

  

- [ ] Implement vector addition on the GPU.

- [ ] Implement a reduction.

- [ ] Implement matrix multiplication.

- [ ] Compare CPU and GPU execution, including transfer time.

- [ ] Profile a kernel and identify memory or compute bottlenecks.

- [ ] Study an NCCL collective such as all-reduce.

  

### Hardware note

  

CUDA requires a supported NVIDIA GPU for hands-on execution. If you do not have one, study the programming model and use a remote GPU environment when practical. For AMD GPUs, investigate [ROCm](https://rocm.docs.amd.com/), but do not try to learn CUDA and ROCm simultaneously at the beginning.

  

### Completion criteria

  

You can explain the GPU execution and memory models, write a small kernel, and measure whether the workload benefits from GPU execution.

  

---

  

## Phase 12 — Datacenter hardware and networking

  

**Estimated duration:** 6-8 weeks  

**Priority:** Strongly recommended for infrastructure engineering

  

This phase zooms out from one server to the systems that connect hundreds or thousands of servers.

  

### Learn

  

**Server and rack architecture**

  

- Server types and motherboard layouts.

- CPU sockets, DIMM population, PCIe topology.

- NVMe storage and storage controllers.

- NICs and network interfaces.

- Rack units and power distribution.

- Redundant power supplies and cooling.

- Failure domains.

  

**Networking**

  

- Ethernet basics and link speeds.

- NIC queues and interrupts.

- TCP/IP fundamentals.

- Switching and routing.

- Top-of-rack switches.

- Leaf-spine topology.

- Network congestion and oversubscription.

- RDMA and InfiniBand concepts.

  

**Storage**

  

- Local NVMe versus network storage.

- RAID concepts.

- Distributed and parallel filesystems.

- Replication and data durability.

  

### Resources

  

- [Linux kernel documentation](https://www.kernel.org/doc/html/latest/)

- [Linux networking documentation](https://www.kernel.org/doc/html/latest/networking/)

- [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/)

- [Computer Networking: Top-Down Approach resources](https://gaia.cs.umass.edu/kurose_ross/index.php)

- [NERSC documentation](https://docs.nersc.gov/)

  

### Projects

  

- [ ] Draw the hardware topology of a server: CPU, RAM, PCIe devices, NICs, and storage.

- [ ] Build a small TCP client-server program using POSIX sockets.

- [ ] Build a small network benchmark using `iperf3`.

- [ ] Measure throughput and latency under different network conditions.

- [ ] Inspect NIC and PCIe device information with `lspci` and `ethtool`.

- [ ] Design a leaf-spine network on paper and explain its traffic paths and failure domains.

- [ ] Compare local NVMe storage with network storage conceptually and, if available, experimentally.

  

### Completion criteria

  

You can explain how compute, memory, storage, and networking connect inside a server and across a datacenter.

  

---

  

## Phase 13 — Distributed systems

  

**Estimated duration:** 8-10 weeks  

**Priority:** Essential for large-scale infrastructure, but conceptually separate from HPC

  

HPC is often about splitting a large computation across processors and nodes. Distributed systems is about building services that continue to behave correctly when machines communicate over networks, slow down, crash, or disagree.

  

### Learn

  

- RPC and serialization.

- Network failures and timeouts.

- Partial failure.

- Logical clocks and ordering.

- Replication and consistency.

- Leader election.

- Consensus and Raft.

- Partitioning and sharding.

- Distributed transactions.

- Fault tolerance and recovery.

- Distributed storage.

- Observability and debugging across services.

  

### Primary resource

  

- [MIT 6.5840 — Distributed Systems](https://pdos.csail.mit.edu/6.5840/)

  

### Supporting resources

  

- [MIT 6.5840 labs and schedule](https://pdos.csail.mit.edu/6.5840/)

- [Google SRE books](https://sre.google/books/)

  

### Projects

  

Build in this order:

  

- [ ] A simple TCP client-server application.

- [ ] A small RPC framework.

- [ ] A single-node key-value store.

- [ ] A replicated key-value store.

- [ ] A Raft implementation or a substantial part of one.

- [ ] Fault-injection tests for node crashes, network delays, and message loss.

- [ ] A technical write-up describing consistency guarantees and failure behavior.

  

### Completion criteria

  

You can explain why distributed systems need explicit approaches to failure, consistency, replication, and coordination. You can demonstrate those ideas in a working project.

  

---

  

## Phase 14 — AI infrastructure specialization

  

**Estimated duration:** 6-10 weeks initially; continue as a specialization  

**Priority:** Optional specialization aligned with AI infrastructure engineering

  

This phase connects systems fundamentals to the infrastructure required to train and serve large models.

  

### Learn

  

**Model execution and resource constraints**

  

- Training versus inference.

- Prefill versus decode.

- GPU memory capacity and bandwidth.

- Batching and continuous batching.

- KV cache and memory pressure.

- Quantization.

- Tensor parallelism and pipeline parallelism.

- Communication overhead between GPUs.

  

**Infrastructure and serving**

  

- Model serving architectures.

- Request scheduling and admission control.

- GPU utilization and observability.

- Model loading and cold starts.

- Multi-GPU communication.

- Autoscaling and load balancing.

- Failure handling and graceful degradation.

  

### Resources

  

- [NVIDIA CUDA Programming Guide](https://docs.nvidia.com/cuda/cuda-programming-guide/index.html)

- [NVIDIA NCCL documentation](https://docs.nvidia.com/deeplearning/nccl/)

- [vLLM documentation](https://docs.vllm.ai/)

- [NVIDIA Triton Inference Server documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/)

- [Ray documentation](https://docs.ray.io/)

- [KServe documentation](https://kserve.github.io/website/)

- [Google SRE books](https://sre.google/books/)

  

### Projects

  

- [ ] Serve a small open model locally or on a rented GPU instance.

- [ ] Measure time-to-first-token, inter-token latency, throughput, and GPU memory use.

- [ ] Compare request batching configurations.

- [ ] Observe how concurrency affects throughput and latency.

- [ ] Deploy a model server behind a simple load balancer.

- [ ] Add metrics, dashboards, and basic failure handling.

- [ ] Write a performance report explaining the bottleneck and trade-offs.

  

### Completion criteria

  

You can explain how model-serving performance depends on compute, memory, batching, scheduling, and communication. You can measure a serving system rather than relying only on framework defaults.

  

---

  

# 4. Capstone projects

  

Complete at least two of these. A strong, measured project is more valuable than several shallow repositories.

  

## Capstone A — NUMA-aware memory benchmark

  

Build a benchmark that compares local and remote memory access on a machine exposing multiple NUMA nodes.

  

Deliverables:

  

- Benchmark source code.

- Reproducible run instructions.

- Hardware and software details.

- Local-versus-remote latency and bandwidth results.

- A topology diagram.

- A report explaining measurement limitations.

  

## Capstone B — CPU performance laboratory

  

Create a suite of small programs that demonstrate cache locality, branch prediction, false sharing, vectorization, and thread scaling.

  

Deliverables:

  

- Source code for each experiment.

- Automated benchmark scripts.

- Raw results and plots.

- A written explanation of each performance effect.

  

## Capstone C — Parallel numerical workload

  

Implement a numerical workload in sequential C, OpenMP, and MPI. Compare performance and scalability.

  

Deliverables:

  

- Sequential baseline.

- OpenMP implementation.

- MPI implementation.

- Optional hybrid implementation.

- Strong- and weak-scaling analysis.

  

## Capstone D — Small distributed storage service

  

Build a replicated key-value store with failure injection and observability.

  

Deliverables:

  

- Architecture diagram.

- Protocol description.

- Replication and recovery behavior.

- Automated tests.

- Latency and throughput benchmarks.

- Failure-analysis report.

  

## Capstone E — AI inference performance study

  

Deploy a model server and investigate how batching, concurrency, and memory pressure affect performance.

  

Deliverables:

  

- Deployment instructions.

- Test workload.

- Metrics and dashboards.

- Performance results.

- Bottleneck analysis.

- Cost and reliability trade-offs.

  

---

  

# 5. Recommended learning sequence at a glance

  

| Order | Phase | Estimated time | Priority |

|---|---|---:|---|

| 0 | Digital logic and computer fundamentals | 2-3 weeks | Foundational |

| 1 | C and systems programming | 6-8 weeks | Essential |

| 2 | Computer architecture | 6-8 weeks | Essential |

| 3 | Operating systems | 8-10 weeks | Essential |

| 4 | Linux internals | 6-8 weeks | Essential |

| 5 | Concurrency | 6-8 weeks | Essential |

| 6 | Performance engineering | 4-6 weeks | Essential |

| 7 | NUMA and multi-socket systems | 4-6 weeks | Core interest |

| 8 | OpenMP | 4-6 weeks | HPC core |

| 9 | MPI | 4-6 weeks | HPC core |

| 10 | HPC cluster operations | 4-6 weeks | Recommended |

| 11 | GPU computing | 6-8 weeks | Specialization |

| 12 | Datacenter architecture | 6-8 weeks | Recommended |

| 13 | Distributed systems | 8-10 weeks | Essential for infra |

| 14 | AI infrastructure | 6-10 weeks | Specialization |

  

The durations overlap in practice because later phases reinforce earlier ones. Do not add every minimum duration mechanically and assume that the result is a fixed graduation date.

  

---

  

# 6. What to study deeply versus what to recognize

  

You cannot master every subfield at once. Use these depth levels to prevent the roadmap from becoming endless.

  

## Deep understanding required

  

- C memory model and pointers.

- CPU and memory hierarchy.

- Virtual memory and processes.

- Linux debugging and observability.

- Threads, synchronization, and atomics.

- Performance measurement.

- NUMA locality and affinity.

- OpenMP and MPI fundamentals.

- Distributed systems failure and consistency concepts.

  

## Working knowledge is enough initially

  

- Detailed micro-op implementation inside a specific CPU.

- Every x86-64 instruction extension.

- Kernel driver development.

- Advanced lock-free algorithms.

- Every MPI feature.

- RDMA verbs programming.

- GPU assembly and advanced compiler internals.

- Physical datacenter power and cooling engineering.

  

Return to these subjects when a project requires them. Do not study them just to complete a checklist.

  

---

  

# 7. Weekly execution template

  

Copy this template into a new weekly note.

  

```markdown

---

type: weekly-study-log

tags:

  - computer-systems

  - learning-log

week_start:

phase:

---

  

# Weekly Systems Study Log

  

## Learning objectives

- [ ] Objective 1

- [ ] Objective 2

- [ ] Objective 3

  

## Study sessions

  

### Session 1

- Topic:

- Resource:

- Notes:

  

### Session 2

- Topic:

- Resource:

- Notes:

  

### Session 3

- Topic:

- Resource:

- Notes:

  

## Practical work

- [ ] Task 1

- [ ] Task 2

  

## What I can now explain

-

  

## What I still do not understand

-

  

## Benchmark or experiment results

- Environment:

- Command:

- Result:

- Interpretation:

  

## Next week's focus

-

```

  

---

  

# 8. Technical experiment report template

  

Use this template for every serious benchmark or systems experiment.

  

```markdown

---

type: experiment-report

tags:

  - benchmarking

  - systems

status: draft

---

  

# Experiment: Name

  

## Question

What am I trying to find out?

  

## Hypothesis

What do I expect, and why?

  

## Environment

- CPU:

- RAM:

- NUMA topology:

- OS and kernel:

- Compiler and version:

- Compiler flags:

  

## Workload

Describe the input, workload size, and relevant settings.

  

## Method

List the exact commands and steps used.

  

## Baseline

Record the original result.

  

## Results

Include raw measurements and plots.

  

## Interpretation

What do the measurements actually show?

  

## Limitations

What could have affected the result? What does the experiment not prove?

  

## Conclusion

Answer the original question.

  

## References

-

```

  

---

  

# 9. Suggested Obsidian vault structure

  

```text

Computer Systems/

├── 00 - Roadmap/

│   ├── Computer Systems - HPC Roadmap.md

│   └── Weekly Logs/

├── 01 - Foundations/

│   ├── Binary and Data Representation.md

│   ├── C Memory Model.md

│   └── Assembly Basics.md

├── 02 - Computer Architecture/

│   ├── CPU Microarchitecture.md

│   ├── Cache Hierarchy.md

│   ├── Virtual Memory Hardware.md

│   └── Cache Coherence.md

├── 03 - Operating Systems/

│   ├── Processes and Threads.md

│   ├── Scheduling.md

│   ├── Virtual Memory.md

│   └── Filesystems.md

├── 04 - Linux Internals/

│   ├── Linux Process Model.md

│   ├── Linux Memory Management.md

│   ├── Linux Observability.md

│   └── Kernel Tracing.md

├── 05 - Concurrency and Performance/

│   ├── Atomics and Memory Ordering.md

│   ├── False Sharing.md

│   ├── Performance Methodology.md

│   └── Benchmark Results/

├── 06 - NUMA and Hardware Topology/

│   ├── NUMA.md

│   ├── CPU Affinity.md

│   ├── Memory Affinity.md

│   └── Topology Experiments/

├── 07 - Parallel Computing and HPC/

│   ├── OpenMP.md

│   ├── MPI.md

│   ├── GPU Computing.md

│   └── Cluster Scheduling.md

├── 08 - Datacenter Infrastructure/

│   ├── Server Architecture.md

│   ├── PCIe and I/O Topology.md

│   ├── Datacenter Networking.md

│   └── Storage Architecture.md

├── 09 - Distributed Systems/

│   ├── Replication.md

│   ├── Consistency Models.md

│   ├── Consensus and Raft.md

│   └── Failure Handling.md

├── 10 - AI Infrastructure/

│   ├── GPU Inference.md

│   ├── KV Cache.md

│   ├── Model Serving.md

│   └── Distributed Inference.md

└── Projects/

    ├── CPU Performance Lab.md

    ├── NUMA Benchmark.md

    ├── Parallel Workload.md

    └── Distributed KV Store.md

```

  

Use Obsidian internal links such as `[[NUMA]]`, `[[Cache Hierarchy]]`, and `[[Performance Methodology]]` to connect concepts across notes.

  

---

  

# 10. First 30 days: exact starting plan

  

Do not try to study the whole roadmap in the first month. Start with C and the relationship between code, memory, and machine instructions.

  

## Week 1 — Data representation and C setup

  

- [ ] Set up Linux, GCC, GDB, and Git.

- [ ] Review binary, hexadecimal, integers, and pointers.

- [ ] Write a binary/hex converter.

- [ ] Compile a C program with `-Wall -Wextra -Wpedantic -g`.

- [ ] Inspect its assembly using Compiler Explorer or `gcc -S`.

  

## Week 2 — Pointers and memory

  

- [ ] Study pointers, arrays, strings, and structs.

- [ ] Draw stack and heap layouts for small programs.

- [ ] Write a dynamic array.

- [ ] Debug an invalid memory access with GDB.

- [ ] Run a program through Valgrind.

  

## Week 3 — Compilation and execution

  

- [ ] Learn preprocessing, compilation, assembly, and linking.

- [ ] Inspect an object file and executable with `readelf` and `objdump`.

- [ ] Learn function calls and stack frames at a basic level.

- [ ] Compare `-O0` and `-O2` output.

- [ ] Explain what changes and what stays the same.

  

## Week 4 — First systems project

  

- [ ] Build a small arena allocator.

- [ ] Add tests for allocation, alignment, and reset behavior.

- [ ] Measure allocation time against a simple `malloc` workload.

- [ ] Write a short report with design decisions and limitations.

- [ ] Review what you still do not understand before starting architecture.

  

**End-of-month deliverable:** A public or private Git repository containing the allocator, tests, build instructions, and a short engineering report.

  

---

  

# 11. What success looks like

  

At the end of this roadmap, you should be able to:

  

- Explain how source code becomes machine instructions.

- Describe how CPU cores, caches, RAM, and I/O work together.

- Understand how an operating system schedules work and manages memory.

- Debug Linux processes using system tools and kernel tracing.

- Write and benchmark concurrent programs.

- Explain NUMA and diagnose local-versus-remote memory behavior.

- Build shared-memory and distributed-memory parallel programs.

- Understand how HPC clusters schedule and execute workloads.

- Explain how servers connect through datacenter networks and storage systems.

- Design and test a basic fault-tolerant distributed service.

- Understand the infrastructure bottlenecks behind AI model serving.

  

The outcome is not a certificate or a long list of technologies. It is the ability to reason about a system from the application down to the hardware, use measurements to test your assumptions, and explain the trade-offs behind an engineering decision.

  

## Start here

  

- [ ] Complete the environment setup.

- [ ] Begin Phase 0 with Nand2Tetris.

- [ ] Start Phase 1 using CS:APP as the main reference.

- [ ] Create the first weekly study log.

- [ ] Do not move to NUMA until the prerequisites are solid.