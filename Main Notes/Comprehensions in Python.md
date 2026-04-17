
Tags:
- [[Python]]
---
- Syntactic sugar that lets us build mapped / filtered lists or dicts or sets from a given iterable

## nested case
**left-right => outside-in**
```python
normal = []
for c in "abc":
    for i in range(3):
        if i % 2 == 0:
            normal.append((c, i))
        
comp = [(c, i) for c in "abc" for i in range(3) if i % 2 == 0]
print(normal == comp) # True
```

---
## References
- [[Fluent Python]]
- https://www.geeksforgeeks.org/python/nested-list-comprehensions-in-python/