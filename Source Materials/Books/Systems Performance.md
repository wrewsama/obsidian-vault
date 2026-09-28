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

---
Source: https://www.goodreads.com/book/show/18058001-systems-performance
