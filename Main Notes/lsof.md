Tags:
- [[Linux]]
- [[Operating Systems]]
---
## What it is
- list open files
- can filter by things like
    - pid (i.e. which files are open in process `pid` )
    - username
    - IP / port (for TCP/UDP connections)

## How it works
- look in `/proc` for the processes' openfile tables
---
## References
- https://man7.org/linux/man-pages/man8/lsof.8.html
- https://ioflood.com/blog/lsof-linux-command/