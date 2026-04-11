Tags:
- [[Operating Systems]]
---
Paging:
- memory context is split into fixed size pages
- there is a page table that maps page number to frame no in physical mem

Segmentation:
- memory context split by segments (data, heap, etc)
- segment table contains base address (physical memory address) and size limit

Segmentation with paging:
- split by segment then by page
- Each segment of each process has its own page table
- Segment table now stores page table base instead of base physical address

---
## References
- 