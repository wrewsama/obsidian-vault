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

Properties: 1. h(key, i) enumerates all possible buckets

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