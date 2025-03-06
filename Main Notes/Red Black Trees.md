Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]
---

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

---
## References
- 