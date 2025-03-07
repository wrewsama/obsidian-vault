Tags:
- [[Operating Systems]]
---
Microkernel:
- user services are separated from kernel services in memory
- Kernel is generally more robust and extensible (more things running in user mode instead of kernel mode)
- Better isolation and protection between kernel and high level services

Monolith:
- user and kernel services share same memory space
- Better performance (no overhead of message passing, provided by OS, between user and kernel services)

---
## References
- 