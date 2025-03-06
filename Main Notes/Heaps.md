Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]
---

Complete binary tree represented as an array

- root is at index 0
- parent index = (idx-1)/2
- left child index = 2*idx + 1
- right child index = 2*idx + 2

insertion:

1. insert new val at leaf
2. bubble up as needed

deletion:

1. swap top with leaf node
2. delete
3. bubble down as needed

heapify:

start at the bottom and call bubble each element down as needed - O(n) can be proven with Taylor series

intuition: want to minimise the distance we need to bubble the lower nodes (bc there are exponentially more nodes at the lower levels) hence bubbledown/siftdown is better than bubble/siftup

---
## References
- 