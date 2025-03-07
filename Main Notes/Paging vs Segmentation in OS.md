Tags:
- [[Operating Systems]]
---
Paging:
- mem context is split into fixed size pages
- there is a page table that maps page no to frame no in physical mem

Segmentation:
- mem context split by segments (data, heap, etc)
- seg table contains base addr in physical mem and size limit

Segmentation with paging:
- split by seg then by page
- Each segment of each process has its own page table
- Segtable now stores page table base instead of base phys addr

---
## References
- 