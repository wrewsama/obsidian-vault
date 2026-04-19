Tags:
- [[Data Structures and Algorithms]]
---
```python
def djikstra(src: int, tgt: int, adjlist: dict) -> int:
    visited = set() # nodes with confirmed shortest dists
    minheap = [(0, src)] # (dist_from_src_to_node, node)
      
    while minheap:  
        cur_dist, cur_node = heappop(minheap)  
        if cur_node in visited: # already got shorter dist, skip
            continue
        visited.add(cur_node)
        if cur_node == tgt:  
            return cur_dist  
          
        for child_node, edge_dist in adjlist[cur_node]:  
            heappush(minheap, (cur_dist+edge_dist, child_node))  
        
    return -1 # cannot reach tgt node
```

## getting path
- include `prev_node` in the minheap
- track `parents` map
- update `parents` when _visiting_ a node
- rebuild path by traversing `parents` from `tgt` to `src`
---
## References
- https://leetcode.com/problems/network-delay-time/