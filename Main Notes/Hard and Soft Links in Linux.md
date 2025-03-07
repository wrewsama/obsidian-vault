Tags:
- [[Operating Systems]]
---

|                       | Hard                                                                                     | Soft                                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Command               | `ln original link`                                                                       | `ln -s original link`                                                                                               |
| Implementation        | Directory entry is also the original file's inode number, reference count is incremented | Is a special file with its own inode that stores the path to the original. The kernel redirects to that stored path |
| Deletion of original  | Hard link will still exist, inode reference count will be decremented                    | Soft link becomes dangling                                                                                          |
| Different filesystems | cannot span different filesystems                                                        | can span different filesystems                                                                                      |

---
## References
- [[Operating Systems Three Easy Pieces]]