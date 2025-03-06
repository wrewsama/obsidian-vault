Tags:
- [[Tags/Databases|Databases]]
---
**Overview**
- Log-Structured Merge Tree
- optimised for fast writes (as opposed to B trees which are optimised for reads)
- K-V writes are batched in a memtable (in memory)
	- usually a balanced binary tree sorted by object key
- once memtable is full, data is flushed to disk as an immutable sorted string table (SSTable)
- periodically, SStables are merged and compacted
	- similar to the merge sort idea since each SStable is sorted
	- deduplicates the keys and keeps the most recent write / tombstone
	- maintains multiple levels of SStables
**Read Process**
- search in memtable
- for each level
	- for each SSTable from most recent to least recent
		- search for the desired key (can use binary search since it's sorted)

**Read Optimisations**
- in-memory summary table to indicate the min/max range of each disk block of every level
	- Saves random IO costs as blocks can be skipped
- Bloom Filter to efficiently minimise having to search every block on every level for a nonexistent key

---
## References
- 