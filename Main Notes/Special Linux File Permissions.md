Tags:
- [[Linux]]
- [[Operating Systems]]
---
## SUID
- set user id
- always executes as the user who owns the file
- affects the user's `x` setting in the output of `ls -l`
    - x + SUID: `s`
    - SUID only: `S`
    - x only: `x`
    - none: `-`
## SGID
- set group id
- on file: file executes as group that owns the file
- on directory: any files created as a _direct child_ of that directory as group ownership set to directory owner
- affects the group's `x` setting in the output of `ls -l`
    - x + SGID: `s`
    - SGID only: `S`
    - x only: `x`
    - none: `-`
## Sticky bit
- doesn't affect files
- for directories, only allows directory owner and root to remove files, regardless of write permissions

---
## References
- https://www.redhat.com/en/blog/suid-sgid-sticky-bit