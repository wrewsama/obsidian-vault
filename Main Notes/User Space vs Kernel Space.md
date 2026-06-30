Tags:
- [[Operating Systems]]
---
## what
- different areas of memory for executing programs
- kernel space: for privileged operations by the kernel (e.g. process and hardware management)
- user space: for normal user programs

## significance in production
- the output of `top`/`htop` will reveal the %cpu spent in user (green) or kernel (red) space
- stuck in kernel space => blocked on syscalls like I/O, [[Page Fault]]s, etc
- stuck in user space => blocked on application logic, e.g. infinite loop
---
## References
- https://www.nrtec.in/wp-content/uploads/2025/03/ertos-unit_2.pdf (10.1.1)
- https://unix.stackexchange.com/questions/698514/what-is-this-color-bar-for-in-htop-page