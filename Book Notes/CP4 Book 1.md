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