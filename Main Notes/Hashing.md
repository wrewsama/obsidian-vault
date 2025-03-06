Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]

---
Open Addressing

Properties:
1. h(key, i) enumerates all possible buckets
2. Every key is equally likely to be mapped to every permutation

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

---
## References
- 
