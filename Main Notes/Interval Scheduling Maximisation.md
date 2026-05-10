Tags:
- [[Data Structures and Algorithms]]
---
- problem: given list of intervals, find max subset of non-overlapping intervals
- solution:
    - sort by _end_
    - greedily take the non-overlapping interval with the smallest end
- $O(N)$
---
## References
- https://www.cs.princeton.edu/~wayne/kleinberg-tardos/pearson/04GreedyAlgorithms-2x2.pdf