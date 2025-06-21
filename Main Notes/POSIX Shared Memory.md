Tags:
- [[Operating Systems]]
- [[Linux]]
---
## What it is
- Region of RAM that can be accessed by different processes
    - by default, processes have their own, isolated memory regions managed by the OS
    - shared memory enables different processes to read and write to each other without expensive syscalls every time
- Legacy Alternative IPC mechanism: [[SystemV Shared Memory]]
- Physically, just a special region of memory, mounted as a [[tmpfs]] filesystem in `/dev/shm`

## Usage
- Create or open shared memory object using `shm_open` to open a shared-memory "file" in `/dev/shm`
- set size with `ftruncate`
- [[mmap]] to map from process's virtual addresses to the physical pages in the shared memory region. This will return a pointer to the start of that virtual address range
- use that pointer to access the shared memory 

---
## References
- 