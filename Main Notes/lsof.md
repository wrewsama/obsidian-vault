Tags:
- [[Linux]]
- [[Operating Systems]]
---
## What it is
- list open files & the processes that opened them
- can filter by things like
    - pid (i.e. which files are open in process `pid` )
    - username
    - IP / port (for TCP/UDP connections)
    - directory / file name

## How it works
- look in `/proc` for the processes' openfile tables

## Usage
```bash
# check processes accessing a directory
lsof +D /path/to/dir

# check processes accessing a file
lsof /path/to/file

# check processes accessing a port
lsof -i :8080
```
---
## References
- https://man7.org/linux/man-pages/man8/lsof.8.html
- https://ioflood.com/blog/lsof-linux-command/