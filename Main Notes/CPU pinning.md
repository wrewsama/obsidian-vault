Tags:
- [[Linux]]
- [[Operating Systems]]
---
## why
- reduce context switching
- improve cache efficiency

## commands
- `isolcpu`: exclude cpu cores from receiving tasks from the OS scheduler
- `taskset`: pin process to a cpu core
- `pthread_setaffinity`: set the cpu affinity of a thread

---
## References
- https://www.linkedin.com/pulse/cpu-pinning-101-how-assigning-cores-can-supercharge-performance-pant-cfkac/
- https://wiki.linuxfoundation.org/realtime/documentation/howto/tools/cpu-partitioning/isolcpus
- https://quickref.me/taskset.html