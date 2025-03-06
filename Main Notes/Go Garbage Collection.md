Tags:
- [[Tags/Go|Go]]
- [[Garbage Collection]]
---
- most memory is allocated on the stack
- Go uses escape analysis to see if an object's lifetime needs to exceed the stack frames' then allocates it to the heap if so

GC impl: non generational, concurrent, tricolor mark&sweep

(non generational bc the assumption that short-lived objs are reclaimed most often doesn't hold bc go would have allocated them to the stack)

steps:
1. stop the world
2. turn on write barrier on heap (allow goroutines to run // w the GC)
3. resume the world
4. begin tricolor mark&sweep algo
	1. all objs are white
	2. scan stacks, global vars & heap pointers to find used objects and mark them as grey, stop goroutine when scanning stack
	3. for each grey obj o, mark their references as grey, then mark o as black, continue until no more grey objs
	4. stop the world
	5. clean up all white nodes
	6. resume

white: candidates for deletion
black: no references to white objects, cannot be deleted
grey: reachable from roots but not scanned for their references yet, also cannot be deleted, will turn black when scanned

the intermediate grey state is needed to allow GC to run without stopping the world
if a new object is created that's reachable from the root, it needs to be grey so we know we need to scan its references (so we don't wrongly delete whatever it's referencing)

---
## References
- 