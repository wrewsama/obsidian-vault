Tags:
- [[Tags/Git|Git]]
---
Object Database: essentially a k-v store for each Object

Object types:
- commits
- trees
- blobs

Blobs: file contents w/o metadata

Tree: essentially the project's directory structure but in tree form (subdirs and files are child nodes), files are leaf nodes with pointers to blobs

Trees are also stored in the object db

---
## References
- 