Tags:
- [[Operating Systems]]
---
**Logical Memory** 
- provides a layer of abstraction that allow programs to access memory without dealing with the actual physical memory locations.
- The OS will:
	- allocate logical address space to the process
	- generate logical addresses while the process is running
- page table tracks the mapping of logical addresses to physical addresses
- Translation Look-Aside Buffer (TLB) caches some of the logical address translations in the page table

**Virtual Memory**
- unused pages are stored in secondary storage acting as virtual memory as a swap file
- page table also tracks if a virtual page is in mem or secondary storage
- virtual mem allows the OS to perform as if it has additional RAM

**Benefits of Logical Memory**
- can use virtual memory
- allows for techniques like paging and segmentation by mapping these non-contiguous memory addresses to contiguous logical addresses for programs to use
- allocates separate logical address space for each process, preventing them from interfering with one another

---
## References
- 