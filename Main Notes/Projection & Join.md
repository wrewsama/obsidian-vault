Tags:
- [[Tags/Databases|Databases]]
---
**Projection**
- sort based dedup (create sorted runs then merge & dedup)
- hash based dedup (recursively hash till whole partition can fit in buffer)

**Join**
- Broadcast hash (if 1 table fits in mem)
- Nested loop
- Index Nested loop
- Sort merge join (create 2 sets of sorted runs then merge)
- hash join (recursively hash till 1 table's partitions all can fit in mem)
---
## References
- 