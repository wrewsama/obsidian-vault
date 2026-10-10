Tags:
- [[SRE]]
- [[Operating Systems]]
---
## Introduction
- system performance goal: improve UX by
    - reducing latency
    - reducing computing cost 
    
## Methodologies
- important metric definitions
    - IOPS: how many data transfer operations per second
    - throughput: how much work done per unit time
    - response time: how long an operation takes to complete
    - latency: how long the operation spends waiting for service
    - utilisation: how busy a resource is
    - saturation: how much work is queued for a resource
- choose when to stop analysis based on % of the performance problem solved (e.g. discovering x% of CPU footprint comes from Y and Z), or weighing the ROI
- knee point: boundary between 2 functions, usually in scalability measurements (linear -> sublinear)
- good methodologies
    - ad-hoc checklist
    - problem statement (question and answer)
        - indicator of performance issue?
        - has system ever performed well?
        - recent changes?
        - latency or runtime issue?
        - others affected?
        - runtime environment?
    - scientific method / diagnosis cycle (hypothesising and experimenting)
    - iterate through tools and the metrics they provide
    - USE method: for each resource, check utilisation, saturation, and errors
    - RED method: for each service, check request rate, errors, and duration (of request processing)
    - workload characterisation: who why what how
    - drill-down analysis: monitoring, identification, analysis OR 5 whys
    - latency analysis: split into smaller components, measure latencies, then recurse on the highest latency components
    - event tracing (e.g. packet inspection)
    - collect baseline statistics, then compare with those during issues
    - static perf tuning
    - cache tuning
    - micro-benchmarking (on simple and artificial workloads)
    - performance mantras: don't do it > do it, but don't do it again > do it less > do it later > do it when they're not looking > do it concurrently > do it more cheaply
- capacity planning
    - resource analysis: determine resource limits by measuring usage of different resources at given loads, then extrapolate to find which will be saturated first
    - factor analysis: measure performance drop when minimising each factor individually, then find the most important factors
    
## Operating Systems
- optimisations to limit overhead of user -> kernel mode context switches
    - user mode syscalls
    - memory mapping
    - kernel bypass
    - kernel mode applications
- key syscalls
    - basic IO: read, write, open, close
    - processes: fork, clone, exec
    - sockets: connect, accept
    - files: stat, mmap
    - others
        - ioctl: miscellaneous actions (e.g. enabling instrumentation)
        - brk: extend heap pointer (for mallocing)
        - futex: fast userspace mutex
- systemd: service manager for Linux
    - can use `systemd-analyze` to check boot time. Useful when optimising the startup time
- Kernel Page Table Isolation (KPTI)
    - reduces performance due to extra CPU cycles and TLB flushing
- Extended Berkeley Packet Filter (BPF)
    - VM that runs in kernel mode, allowing user mode BPF tools for tracing, networking, or security

## Observability Tools
- fixed counters: counters maintained by the kernel
    - system-wide tools: vmstat, mpstat, iostat, nstat, sar
    - per-process tools: ps, top, pmap
- profiling: collection of a set of samples of behaviour
    - system-wide tools: perf, profile
    - per-process tools: gprof, cachegrind, language-specific profilers e.g. JFR for Java
- tracing: instrumenting occurrences of events
    - system-wide tools: tcpdump, biosnoop, execsnoop, perf, ftrace, bcc, bpftrace
    - per-process tools: strace, gdb
- monitoring
    - sar, Simple Network Management Protocol (SNMP), agents e.g. Prometheus, collectd

## Applications
> Performance is best tuned closest to where the work is performed: in the applications 
- key ideas
    - optimise the common case (paths that frequently use CPU for CPU bound apps, similar for IO bound apps)
    - ensure applications have good observability
- techniques
    - tune IO size
    - caching reads
    - buffering writes
    - polling: `epoll` instead of `poll`
    - use concurrency and/or parallelism
    - nonblocking IO
    - processor binding (especially with NUMA environments)

## CPUs
- first target for systems performance analysis
- newer CPUs + newer compilers that take advantage of the new instruction sets can result in significant application performance boosts
- methodology: performance monitoring -> USE method -> profiling -> micro-benchmarking -> static perf tuning
- experimentation tools
    - `mpstat` + infinite loop in bash `while :; do :; done &`
    - `sysbench`
- tuning
    - compiler optimisation options
    - priority (niceness)
    - scheduler options
    - governors (control CPU clock frequencies)
    - power states (sleep states trade latency for power efficiency)
    - CPU binding with `taskset` / `numactl`
    - exclusive CPU sets (similar to binding but also automatically prevents other process from getting scheduled on those CPUs)
    - resource controls (e.g. cgroups)
    - security boot options (disabling some can improve performance, but this is NOT RECOMMENDED)
    - BIOS tuning (e.g. disabling turbo boost during benchmarking to ensure consistent clock frequency)

## Memory
- demand paging
    - `malloc` causes unallocated virtual memory to be allocated (but not mapped to physical memory yet)
    - when storing something in that virtual memory space, lookup is done on the MMU
    - if page has a physical mapping, access that (may be in memory or may have been swapped out due to memory pressure)
    - if no mapping, page fault
        - if data is on a physical memory page, create mapping to it (minor page fault)
        - else, load from disk (major page fault)
- memory <> cpu architecture
    - UMA: all CPUs --system bus-> all DRAM
    - NUMA: each CPU --memory bus-> 1 unit of DRAM, CPUs are connected via CPU interconnect
- methodology: performance monitoring -> USE method -> characterising usage
- tuning
    - page sizes (e.g. hugepages)
    - memory allocators
    - NUMA bindings
    - resource controls (e.g. `ulimit`)

## File Systems
- NOTE: file system performance != disk performance
- file systems include the syscall interface and the file system cache
- Virtual File System (VFS): facade for different file systems and their interfaces
- methodology: latency analysis -> performance monitoring -> workload characterisation -> micro-benchmarking -> static performance tuning
- experimentation
    - remember to flush file system caches beforehand
    - ad hoc `dd`
    - micro-benchmarking tools e.g. `fio` or `sysbench`
- tuning
    - improve application calls (e.g. `fsync`ing batched writes)
    - filesystem-specific options (e.g. disabling the access time on `ext4` mounts)

## Disks
- architecture: 
    - CPUs <-IO Bus-> Disk Controller <-Storage Bus-> Disk Devices
    - each disk device has a on-disk cache and an I/O queue (requests hit cache first, misses get queued)
- Linux optimisations
    - LInux merges / coalesces IO requests before queuing them up
    - IO schedulers reorder IO requests in the queue for optimised delivery
- methodology: USE method -> performance monitoring -> workload characterisation -> latency analysis -> micro-benchmarking -> static analysis -> event tracing
- experimentation
    - ad hoc `dd`
    - custom load generators (just open device path and do work)
    - micro-benchmark tools: `hdparm`, `ioping`, `fio` with non-buffered IO
- tuning
    - OS tunables e.g. `ionice`, cgroups, `/sys/block` parameters (e.g. IO scheduler policy)
    - device tunables (e.g. power management)
    - disk controller tunables

## Network
- Linux kernel network stack architecture
    - syscall interface
    - socket buffers (send / recv)
    - transport layer
    - network layer
    - queuing discipline (`qdist`)
    - NIC device drivers (e.g. `ena`, `ixgbe`) 
- linux optimisations
    - connection queues for burst handling
    - send/receive buffering
    - segmentation offload (letting NIC do the segmentation)
    - queuing discipline to control the scheduling of packets
    - CPU scaling (multiprocessing packets)
- methodology: performance monitoring -> USE method -> static performance tuning -> workload characterisation
- experimentation
    - benchmarking: `ping`, `traceroute`, `pathchar`, `iperf`, `netperf`
    - traffic control (through manipulating `qdisc`): `tc`
- tuning
    - buffers: socket, tcp
    - backlogs: TCP, device
    - congestion control
    - misc TCP options e.g. SACK and FACK
    - IP Explicit Congestion Notification (ECN)
    - resource control with cgroups
    - queuing disciplines
    - socket options

## Cloud Computing
- hardware virtualisation: VMs with their own kernels, on top of a hypervisor
    - overhead
        - translation between guest and hypervisor to execute CPU instructions
        - memory mapping translation (though the TLB can cache it)
            - virtual to guest-physical
            - guest-physical to host-physical
        - IO translation (can be avoided with PCI pass-through)
    - observability: since you have a guest kernel, just use the usual kernel-based observability tools
- OS virtualisation: partition OS into containers
    - implemented using namespaces and cgroups
    - overhead
        - possible CPU contention with other tenants on the host, no other overhead as the container threads run directly on the real CPUs
        - similarly, no overhead for memory mapping
        - IO (file system and network) overhead to ensure isolation
    - observability
        - normal observability tools (no per-container view)
        - cgroup statistics (if you can figure out which cgroup your container is in)
        - namespace mapping (similar to the above)
        - container tools (e.g. `kubectl top`)
- lightweight virtualisation: best of both worlds; VMs with their own kernels on top of a _lightweight hypervisor_ (much fewer supported devices e.g. video, audio, PCI bus)
    - e.g. Amazon Firecracker
    - overhead is similar to hardware virtualisation, but with much lower memory footprint (since the hypervisor is much smaller)
    - observability same as hardware virtualisation

## Benchmarking
- characteristics of a good benchmark
    - repeatable
    - observable
    - portable
    - easily presented
    - realistic
    - runnable
- benchmarking types
    - micro-benchmarking: artificial workload for single operation type
    - simulation (aka macro-benchmarking): artificial workload to mimic production
    - replay: rerun events captured in production trace logs
    - industry-standard benchmarks

## perf
- `perf stat`: counts events
    - `-e` option is used to search for events, supports globs
    - `-I` option groups counts into intervals
    - `-A` groups counts by CPU
- `perf record`: records events to a `perf.data` file
    - `-e` option same
- `perf report`: summarises content of `perf.data` file
    - TUI (default)
    - stdio (use `--stdio` flag)
- `perf script`: prints samples from `perf.data`
    - useful for constructing flame graphs
- `perf trace`: trace system calls and print output to stdout
---
Source: https://www.goodreads.com/book/show/18058001-systems-performance (2nd edition)
