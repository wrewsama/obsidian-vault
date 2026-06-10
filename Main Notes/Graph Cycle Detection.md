Tags:
- [[Data Structures and Algorithms]]
---
## intuition
- tricolour DFS
- each node is white initially
- when entering, colour the node grey
- when exiting, colour the node black
- if you traverse back to an already grey node, you know there's a cycle
- reason for black nodes: in a directed graph, you might have 2 completely different paths that reach the same node, which isn't a cycle

## undirected case
- the case for the black nodes mentioned above won't happen in an undirected graph
- as long as you touch the same node twice, that's a cycle
- only need black and white => a simple `visited` set will do
---
## References
- https://cp-algorithms.com/graph/finding-cycle.html