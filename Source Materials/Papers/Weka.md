Tags:
- [[Distributed Systems]]
---
## What it is
- file system offering a high performance, file-based storage solution
- software running on AMD / x86 servers with commodity NVME SSDs
- parallel, horizontally scalable, and fast

## Architecture
- File Services: frontend interfaces
    - NFS
    - S3
    - etc.
- Compute and clustering: backend manager for data distribution and metadata
- SSD Drive agent: turns SSD into an efficient network device
- Management process: manage events, CLI, stats
- Object connector: handles reads and writes to object store

---
Source: https://www.weka.io/wp-content/uploads/resources/2023/03/weka-architecture-white-paper.pdf
