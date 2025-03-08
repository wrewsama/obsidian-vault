Tags:
- [[Data Structures and Algorithms]]
---
## Chapter 1
- computers can process around 10^8 operations in 1 second
- analyse algo before starting to code

## Chapter 2
special sorting problems
- count inversions 
- linear time sorting
	- counting
	- radix

important data structures
- linked list
- stack
- queue
- deque
- binary heap / priority queue
- hashtable
- bBST (`sortedcontainers` isn't in python stdlib)
- Order Statistics Tree: dynamic kth smallest element
- union find
- fenwick tree: dynamic range sum & queries
	- Range Update Range Query fenwick tree
- segment tree: dynamic range queries

## Chapter 3
problem solving paradigms:
- complete search
	- nested loops (possibly with pruning)
	- backtracking
	- tips
		- gradually generate solutions and prune invalid partial solutions instead of generating all possible candidates and filtering them
		- use symmetries
		- pre-calculate results
		- solve problem backwards
		- compress data
- Divide and Conquer
	- steps
		- divide problem into subproblems
		- solve subproblems
		- if needed, combine subproblems to get complete solution
- Greedy
	- required properties
		- has optimal sub-structures
		- has a greedy property
- DP

## Chapter 4
graph problems
- traversals
	- DFS
	- BFS
	- Finding Connected Components / Flood fill
	- topological sort
		- reverse postorder DFS
		- Kahn's algo
	- Bipartite graph check
	- Cycle check
	- Finding articulation points and bridges
		- articulation point: vertex in G whose removal makes G disconnected
		- bridge: edge in G whose removal makes G disconnected
		- DFS with `dfs_num`(iteration counter) and `dfs_low`(minimum reachable `dfs_num`) for each node
	- Find Strongly Connected Components
		- Kosaraju's
		- Tarjan's
- MST
	- Kruskal's
	- Prim's
	- other applications
		- max spanning tree: negate and Prim's
		- min spanning subgraph (MST but some edges are fixed): set fixed edge priorities to infinite then Prim's
		- min spanning forest of K connected components: Get MST then remove biggest K-1 longest edges
		- minimax (find the min max edge weight among all possible paths between `i` and `j`): The MST's path between `i` and `j` is the one with the min max edge weight
		- 2nd best spanning tree: need to replace one of the edges, try blocking each edge and running Kruskal's
- SSSP
	- unweighted: BFS
	- no negative cycles: Dijkstra's (push to pq for every edge, don't skip repeated node if distance is less than previously found)
	- with negative cycle: Bellman-Ford (stop after V-1 outer loop iterations, can detect negative cycles by running 1 last time and checking if anything was modified)
- APSP
	- Floyd-Warshall
- Special graphs
	- DAG
		- DAG-SSSP: relax edges in topological order
		- counting paths
	- Tree
		- traversal: BFS/DFS
		- articulation points and bridges: all internal vertices and all edges
		- SSSP: only 1 possible path, just BFS/DFS
		- APSP: do above SSSP for every vertex
		- diameter (greatest shortest path) of weighted tree: find furthest vertex `x` with BFS from any node, then BFS from `x` to find its furthest vertex `y`. `x` to `y` is the diameter of the tree
	- Bipartite graph
		- Max Cardinality Bipartite Matching
	- Eulerian Graph (contains Eulerian tour i.e. visit each vertex once)
		- checking if graph is Eulerian
			- connected
			- all vertices have even degree (undirected) or all vertices have indegree == outdegree
				- for undirected, if exactly 2 vertices have odd degree, there is an Eulerian path between those 2
				- for directed, if exactly 1 vertex has 1 extra indegree and 1 vertex has 1 extra outdegree, there is an Eulerian path between those 2
		- find Eulerian Path
			- Hierholzer's algo