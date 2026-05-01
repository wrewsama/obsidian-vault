Tags:
- [[Computer Architecture]]
- [[Operating Systems]]
---
- Cache organisation method 
    - cache is divided in sets
    - each set contains multiple lines
    - each memory address block can be mapped to a set, and it can be cached in any line within that set
- `N-Way Set Associative` => each set has N lines
- `Direct-Mapped` => each set has 1 line (1-way set associative)
- `Fully Associative` => 1 set with as many lines as can fit in the cache
---
## References
- https://quicksilicon.in/blog/cache-associativity-variants-importance-of-cache-associativity