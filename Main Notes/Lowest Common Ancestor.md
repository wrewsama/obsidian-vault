Tags:
- [[Data Structures and Algorithms]]
---
## Intuition
#### Pre-processing
There are 2 main parts to the preprocessing: `up` and `isAncestor`

**`up`**
- `up[node][i]` = the ancestor $2^i$ edges above `node`
- obviously, `i` <= `maxlevel` = `ceil(log2(n))`
- as we traverse the graph in `dfs`, we set all the values in `up[node]`
- $2^i$th ancestor of `node` = $2^{i-1}$ th ancestor of (`node`'s $2^{i-1}$ th ancestor), which would have been processed earlier in the `dfs`

**`isAncestor`**
- treat every time we enter and exit a node as a new event
- using `self.t, tin, tout`, we have a unique timestamp for each enter and exit event
- if `x` is an ancestor of `y`, we know we must have the events `enter(x), enter(y), exit(y), exit(x)` in that relative order

#### Finding the LCA
trivial cases: if either one is an ancestor of the other

**binary search**
want to find the highest ancestor of x that _ISN'T_ an ancestor of y. The LCA will just be the 1st ancestor (parent) of that

- we know the search window halves at each step, this corresponds to a decrement in `i`
- start at `up[x][maxlevel]` (this will just be root, which will always be ancestor of any node)
- think of that as the `mid` in a standard binary search
- if it is an ancestor of `y`, just continue
    - next iteration will decrement `i`, so we're essentially setting `mid` to the mid of the lower half
- else, set `cur` to `up[x][i]`
    - next iteration will decrement `i`, so we're going to `mid`, then setting it to the mid of the upper half

## Implementation
```python
maxlevel = ceil(log2(n))
self.t, tin, tout = 0, [-1 for _ in range(n)], [-1 for _ in range(n)]
up = [[0 for _ in range(maxlevel+1)] for _ in range(n)]

def dfs(node, parent):
    self.t += 1
    tin[node] = self.t
    up[node][0] = parent
    for i in range(1, maxlevel+1):
        up[node][i] = up[up[node][i-1]][i-1]
    
    for child, _ in adjList[node]:
        if child == parent:
            continue
        dfs(child, node)
    self.t += 1
    tout[node] = self.t

def isAncestor(x, y):
    return tin[x] <= tin[y] and tout[x] >= tout[y]

def lca(x, y):
    if isAncestor(x, y):
        return x
    if isAncestor(y, x):
        return y

    cur = x
    for i in range(maxlevel, -1, -1):
        if isAncestor(up[cur][i], y):
            continue
        cur = up[cur][i]

    return up[cur][0]

dfs(0, 0)
```
---
## References
- https://cp-algorithms.com/graph/lca_binary_lifting.html
- https://leetcode.com/problems/minimum-weighted-subgraph-with-the-required-paths-ii/submissions/1637074692/