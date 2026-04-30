Tags:
- [[Operating Systems]]
---
## what
- `malloc`: memory allocator, gives the request amount of data from the heap
    - note: NOT a syscall, it's part of `libc`
- `free`: deallocate memory provided by `malloc`
    - also not a syscall, also partt of `libc`

## how
- allocator tracks the free parts of the heap
    - e.g. in buddy blocks, free list, bitmap, etc.
- `malloc(n)` looks for an free block of `n` (+ some for the header) bytes and allocates it
- `free` marks the block as freed, merging it with adjacent free blocks
- each allocated block has a header including
    - size of the block
    - flag marking if the block has been freed
    - (optional) `magic`: a known "magic number" for corruption detection
        - if the block's `magic` field isn't equal to the known magic number, we know the block is corrupt (e.g. buffer overflow from previous block)
---
## References
- https://medium.com/@varshilshah.in/the-secret-life-of-malloc-and-free-c00c9db555f9