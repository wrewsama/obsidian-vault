Tags:
- [[Operating Systems]]
---
- Heap
	- stores dynamically allocated objects
	- can be allocated and freed at will (lifetime not tied to a stack frame)
    - slower allocations and frees as it requires finding an empty space (in logical memory) and handling fragmentation
- Stack
	- stores local variables(in stack memory) & method call info
	- when functions are called, a new block is automatically created on top of the stack containing relevant values
    - when function exits, block is automatically freed
    - can be allocated / freed quickly as there's no overhead of looking for a place to put it (it's just pop last or push on top of last)

---
## References
- https://www.educative.io/blog/stack-vs-heap