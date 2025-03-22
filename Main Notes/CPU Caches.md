Tags:
- [[Computer Architecture]]
---
## L1
- Smallest and fastest
- Each CPU core has its own
- Stored on the core itself
- Split into L1i and L1d caches to store instructions and data respectively

## L2
- mid size and speed
- stores both instructions and data
- Can be shared or per-core, depending on processor

## L3
- larger and slower (compared to the other 2)
- shared across CPU cores, stored outside the cores on the CPU chip

## Usage
* For reads, the cache hierarchy is checked in order until desired data is found
* For writes, cache updates depend on write policies (write-back or write-through)
---
## References
- https://dev.to/larapulse/cpu-cache-basics-57ej
- https://www.howtogeek.com/891526/l1-vs-l2-vs-l3-cache/