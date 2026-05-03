Tags:
- [[Python]]
---
## what
- Objects are allocated to a private heap, managed by the **Python Memory Manager**
- 3 levels of allocators (highest to lowest)
    - object-specific allocators different primitive object types (e.g. ints, strings, etc.)
    - python's object allocator (default = `pymalloc`)
    - raw memory allocator (requests memory straight from the OS, similar to the good old `malloc` / `free`)

## memory allocation flow
- object has a object-specific allocator for that type => use that
- object <= 512B => use object allocator (default pymalloc)
- object > 512B => use raw memory allocator

## pymalloc
- manages large regions of memory called arenas
- arenas > pools > blocks
- objects are stored in a single block, the block's size must be a multiple of 8B
- each pool stores and manages blocks of a certain size
- CPython will only request and free memory to/from the OS by arenas
    - minimises repeated syscalls

---
## References
- https://www.honeybadger.io/blog/memory-management-in-python/#memory-allocators