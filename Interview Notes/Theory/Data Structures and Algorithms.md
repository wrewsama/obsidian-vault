## B Trees

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

## Red Black Trees

Properties:

- each node is either red or black
- root and leaves(null) are black
- if node is red, then children are black
- all paths from node to leaf contain same number of black nodes

- longest path cannot be more than 2x length of shortest path (longest path will alternate red and black while shortest will have only black)

insertions:

- insert node and colour it red
- recolour and rotate nodes to fix violations. Do the following recursively:

- node is the root: colour it black
- node's uncle (parent's sibling lol) is red: recolour parent, uncle, and grandparent
- node's uncle is black

- node is near side (closer to uncle): rotate parent to far side
- node is far side: rotate grandparent to near side then recolour initial parent and grandparent

deletions:

- transplant(u, v) that handles reconnecting parent of deleted node with given child node
- delete has 3 cases

- one child null: note original colour, transplant, then fixup
- neither null: get min node in right subtree (successor), transplant that and its right child, set successor's right child to deleted node's right child

note that successor cannot have a left child as it contradicts the property that the successor is the min val in that subtree

## Hashing

Open Addressing

Properties:
1. h(key, i) enumerates all possible buckets
2. Every key is equally likely to be mapped to every permutation

(linear probing doesn’t do this bc clusters have higher prob)

Still ok thanks to caching

Cost: O(1 / (1 - items/buckets))

Double Hashing

using another hash function to get the jump interval

h(k, i) = f(k) + ig(k) mod m

If g(k) must be relatively prime to m, prop 1 achieved

Quad probing

To terminate, n/m < 0.5 and mod prime num

Table Resizing

N = m -> m = 2m (grow) && n < m/4 -> m = m/2 (shrink)

Growing cost: O(old size + new size + no of elem)

## Heaps

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

## All Pairs Shortest Paths (Floyd-Warshall)

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

## Knuth Morris Pratt (KMP) Pattern matching
Idea:
For each index `i` in the pattern, consider the subarray ending at `i`
Let `lps[i]` be the longest prefix that is also a suffix of that subarray

Now let `p1` be a pointer to the main string and `p2` be a pointer to our pattern.
When we get a mismatch, we now only need to make `p2` jump back to `lps[p2-1]`, because we know that `pattern[:that index]` will be the same as whatever we matched earlier in the main string (cause that's a suffix of `pattern[:p2]`.
Hence, we never have to move p1 backwards so time complexity = `O(len(string) + len(pattern))`

Code:
```python
def strStr(self, haystack: str, needle: str) -> int:
	lps = [0 for _ in range(len(needle))]
	ptr, prevlps = 1, 0
	while ptr < len(needle):
		if needle[ptr] == needle[prevlps]:
			prevlps += 1
			lps[ptr] = prevlps
			ptr += 1
		elif prevlps == 0:
			lps[ptr] = prevlps
			ptr += 1
		else:
			prevlps = lps[prevlps-1]
	
	ph, pn = 0, 0
	while ph < len(haystack):
		if needle[pn] == haystack[ph]:
			ph += 1
			pn += 1
		elif pn == 0:
			ph += 1
		else:
			pn = lps[pn-1]

		if pn == len(needle):
			return ph - len(needle)

	return -1
```

The first part can be extended to find the longest prefix in `word` that's a suffix in `target`
Technique (replace the $ with any character that's guaranteed to not appear in target): 
```python
genlps(word + '$' + target)
```