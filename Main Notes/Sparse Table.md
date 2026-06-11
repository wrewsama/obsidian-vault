Tags:
- [[Data Structures and Algorithms]]
---
> **Key Intuition**: Every number $x$ can be expressed as the sum of at most $log_2{x}$ powers of 2. Hence, range queries can be expressed as the sum of $log_2{n}$ ranges with sizes equal to powers of 2

## data structure
- 2d array `st[i][j]`
- `st[i]` stores results for all ranges of length $2^i$
- `st[i][j]` is the result for the range starting at `j` with length $2^i$

## construction
- use dp
```python
# function is the function you want to range query (e.g. sum, min)
st[i][j] = function(st[i-1][j], st[i-1][j + 2**(i-1)]
```

## query
(using range min as an example)
```python
# max_i is around log2(len(array))
for i in range(max_i, -1, -1):
    if i > (end-start+1):
        continue
        
    res = min(res, st[i][start])
    start += 2**i
```

intuition: think of how you convert numbers to binary from left-to-right: 
- get largest power-of-2 that's smaller than or equal
- chop it off
- repeat on the remaining

---
## References
- https://cp-algorithms.com/data_structures/sparse-table.html