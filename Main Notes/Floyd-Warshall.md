Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]
---

Idea:
if you have the shortest path from i to j using vertices from {1, 2, ... k},
the shortest path from i to k using vertices from {1, 2, ... k+1} will be the min of the
* shortest path without using k+1
* shortest path from i to k+1 plus the shortest path from k+1 to j

code:
```python
# set up the dist matrix with the edge weights
# leave dist as inf for nodes that don't have a path yet
for k in range(nV):
	for i in range(nV):
		for j in range(nV):
			distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])
```

---
## References
- 