Tags:
- [[Data Structures and Algorithms]]
---
## Smallest Value Passing Condition
```python
while lo < hi:
    mid = (lo + hi) // 2
    if condition(mid):
        hi = mid
    else:
        lo = mid + 1
return lo
```

## Largest Value Failing Condition
```python
while lo < hi:
    mid = (lo + hi + 1) // 2
    if condition(mid):
        hi = mid - 1
    else:
        lo = mid
```
---
## References
-  [1](https://leetcode.com/problems/find-k-th-smallest-pair-distance/discuss/769705/Python-Clear-explanation-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)
- [2](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/solutions/1575980/python-binary-search-check-answer-greedily/?envType=daily-question&envId=2025-05-01)