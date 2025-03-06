Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]
---

Properties:

- All leaves are @ the same level (perfectly balanced)
- All non root nodes must be at least half full (floor div by 2)
- For each node, no. of children = no. of keys + 1
- within the node, keys are in ascending order
- O(logn) search insert and delete
- all insertions happen at the leaf node

searches are basically the same idea as a normal ass binary search tree

Insert:

- search for the leaf node where the val should be inserted
- if node is full, split and promote the middle node
- do that recursively to the root

Delete:

- search for the node then delete it
- if that node is now too empty

- if sibling node has extra keys, rotate from that node
- else bring parent key down to replace the deleted key, then merge the node that is also a child of that key

- for internal node, just replace deleted val with an appropriate child val (max of left or min of right) then rebalance with the above steps

---
## References
- 