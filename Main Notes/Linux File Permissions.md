Tags:
- [[Linux]]
- [[Operating Systems]]
---
- 3 basic permission types
- 3 [[Special Linux File Permissions]]
- different set of options for owner / group / others
    - set it with `chmod` with 3 octal digits representing owner / group /others
    - each digit is 3 bits for read / write / execute
    - not to be confused with the output of `ls -l`, which includes special bits in the output (e.g. the value of the 3rd character in the group section depends on both the group execute bit as well as the special SGID bit)

|           | read          | write                 | execute                                                                    |
| --------- | ------------- | --------------------- | -------------------------------------------------------------------------- |
| file      | -             | -                     | -                                                                          |
| directory | list contents | add / remove contents | access detailed info of contents(e.g. metadata, permissions) or _traverse_ |
_traverse_: entering with `cd` or accessing some subdirectory inside

---
## References
- 