Tags:
- [[Data Structures and Algorithms]]
---
## what
- data structure to efficiently track and merge disjoint sets of objects when given only direct links between pairs of objects
- i.e. given edges, need _connected_ groups (any path, not just edge)
## example
```python
class UnionFind:  
    def __init__(self, n: int):  
        self.parents = {i: i for i in range(n)}  
        self.sizes = {i: 1 for i in range(n)}  
      
    def find(self, n: int) -> int:  
        if self.parents[n] != n:
            # optimisation: path compression
            self.parents[n] = self.find(self.parents[n]) 
        return self.parents[n]  
      
    def union(self, n1: int, n2: int):  
        p1, p2 = self.find(n1), self.find(n2)  
        if p1 == p2:  
            return  
            
        # optimisation: weighted union
        if self.sizes[p1] > self.sizes[p2]:  
            self.sizes[p1] += self.sizes[p2]  
            self.parents[p2] = p1  
        else:  
            self.sizes[p2] += self.sizes[p1]  
            self.parents[p1] = p2
```
---
## References
- 