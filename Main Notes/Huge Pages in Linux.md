Tags:
- [[Linux]]
- [[Operating Systems]]
---
- memory pages larger than the normal 4KB page
- decreases page table size and improves TLB hit rate (more accesses to the same few pages)
- can be both
    - manually configured
        - set number of huge pages available using `sysctl`
        - use them explicitly in the application with the `MAP_HUGETLB` flag on [[mmap]]
    - transparent
        - OS automatically uses it to optimise applications
        - can disable this by modifying `/sys/`
---
## References
- 