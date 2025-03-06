Tags:
- [[Tags/Java|Java]]
- [[Garbage Collection]]
---
2 Generations in the heap: Young(aka Eden space and Survivor spaces 1 and 2) and Old(aka Tenured)

Minor GC runs when eden space is full

Survivors from eden space and prev survivor space go to the other survivor space

When object survives a certain number of minor GC cycles, it gets promoted to old gen

When old gen is full, major GC is triggered

GC is done using mark and sweep

3 types: serial (single thread), concurrent (thread that performs GC along with the app), parallel (uses multiple cores for GC, is stop-the-world)

---
## References
- 