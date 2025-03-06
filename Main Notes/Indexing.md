Tags:
- [[Tags/Databases|Databases]]
---
InnoDB uses B+ tree

advantage over BST: shorter height => fewer disk lookups => better performance
also, each node fits in 1 disk page so scanning multiple pages just for 1 node is not required

advantage over hash index: allows for range queries

properties:

- perfectly balanced (every leaf node at same height)
- every node other than root is at least half full
- every inner node with k keys has k+1 non null children
- for B+ tree: leaf nodes contain tuples' record IDs and pointers to the next and prev nodes

Clustered index: order of rows in data pages corresponds to order of rows in the index
^can only have 1 per table

Sparse index: index records exist for only a subset of the search-keys in the data. Only useful with a clustered index

**Matching Predicate**
B+ Tree
![[Pasted image 20240711211742.png]]

Hash
![[Pasted image 20240711211801.png]]

**Covering Index**
I is a CI for Q if all attributes referenced in Q are part of the key or include columns of I
- no RID lookup
- aka index-only plan
---
## References
- [[High Performance MySQL]]