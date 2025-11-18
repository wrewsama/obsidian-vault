Tags:
- [[Linux]]
---
- defines which permissions to NOT inherit from parent dir
- expressed as 3 octal digits (same as [[Linux File Permissions]])
- $perms_{new} = perms_{parent} \land \neg umask$
    - e.g. if the parent directory has permissions `777` with umask `021`, a new file created under the parent will have permissions `756`
- can be checked with `umask`
- can be set with `umask XXX`
    - applies to current process
    - child processes inherit the umask of their parent
---
## References
- https://www.geeksforgeeks.org/linux-unix/umask-command-in-linux-with-examples/