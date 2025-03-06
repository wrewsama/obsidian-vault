Tags:
- [[Python]]
- [[Garbage Collection]]
---
Python uses a combination of 2 strategies: reference counting and generational mark & sweep

**reference counting**
* Every value is represented as an object with an associated reference count
- Once it reaches 0, it is immediately freed

**Generational mark and sweep**
- helps to find and free cyclic references
- objects are divided into 3 generations
	- young, middle, old
- once a generation reaches a certain threshold, a GC process is triggered on that generation
- traces all objects starting from the root references (whatever's in stack or data memory) and marks them as reachable, free anything not marked as reachable
- any survivors in that generation get promoted to an older generation (unless they're already in the old gen)

random note: `gc.collect()` runs GC on all gens
another note: thanks to the [[GIL]], all GC is stop-the-world

---
## References
- 