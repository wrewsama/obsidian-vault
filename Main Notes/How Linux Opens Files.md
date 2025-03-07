Tags:
- [[Operating Systems]]
---
- kernel receives `open` syscall
- kernel looks up the directory entries along the provided path to find the inode corresponding to the file
    - for each file/directory in the path, use the inode to get the data, then if it's a directory, use the data to get the inode for the next subdirectory / file
    - if the file/directory does not exist, may need to create inode / file by allocating free space using the inode and data bitmaps
- kernel creates an entry in the system-wide open file table
	- tracks state e.g.
	- current offset
	- access flags (e.g. NONBLOCK)
- kernel creates an entry in the process's open file table (aka the file descriptor table) and returns the index of that entry, aka the _file descriptor_

---
## References
- [[Operating Systems Three Easy Pieces]]