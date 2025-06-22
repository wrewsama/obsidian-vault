Tags:
- [[Operating Systems]]
---
## What is it
- syscall that maps parts of a file into memory
- file data is mapped to virtual addresses, which can be accessed via pointers, which loads the data into physical memory without `read`/`write` syscalls => better performance

## How to use it
- function signature: `void *mmap(void *addr, size_t length, int prot, int flags, int fildes, off_t offset);` returning a void pointer indicating the start of the virtual address space
- `prot`: memory protection flags
    - `PROT_NONE`
    - `PROT_READ`
    - `PROT_WRITE`
    - `PROT_EXEC`
    - self-explanatory permissions
- `flags`: mapping types
    - `MAP_SHARED`: write changes back to file (flush with `msync()`), used for IPC
    - `MAP_PRIVATE`: updates not written back to file
    - etc.

## Example
```c
char *map = mmap(0, textsize, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
if (map == MAP_FAILED) {
    perror("mmap");
    close(fd);
    return 1;
}
```

---
## References
- https://man7.org/linux/man-pages/man2/mmap.2.html
- https://www.baeldung.com/linux/memory-mapped-vs-system-call
- https://programmingappliedai.substack.com/p/what-is-mmap-in-linux-and-how-it