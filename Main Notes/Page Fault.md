Tags:
- [[Operating Systems]]
- [[SRE]]
---
## what
- when requested page is not in physical memory (i.e. in swap memory)
- detected by looking at the present bit in the page table entry
## process
- trap to OS
- find the swap file (identified in the page table entry)
- allocate a frame in physical RAM (based on allocation policy e.g. clock, LRU)
- fetch the data
- update the PTE (mapping, present bit)

## troubleshooting 
- slow process, increased latency
- can diagnose using `htop`/`top` with the `MAJFLT` column

---
## References
- [[Operating Systems Three Easy Pieces]] 
- https://man7.org/linux/man-pages/man1/htop.1.html