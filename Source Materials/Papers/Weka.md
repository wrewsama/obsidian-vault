Tags:
- [[Distributed Systems]]
---
## What it is
- file system offering a high performance, file-based storage solution
- software running on AMD / x86 servers with commodity NVME SSDs
- parallel, horizontally scalable, and fast

## Architecture

```
    User                Kernel      
   ┌───────────┐       ┌─────┐      
   │application┼───────►VFS  │      
   └───────────┘       └─┬───┘      
  ┌────────────┐       ┌─▼─────────┐
  │WEKA client ◄───────┼WEKA driver│
  └──────┬─────┘       └───────────┘
         │                          
         │                          
         │ DPDK                     
         │                          
  ┌──────▼──────┐                   
  │Compute Node │                   
  └──────┬──────┘                   
         │load balanced query       
 ┌───────▼────┐                     
 │ Drive Node │                     
 └────┬───────┘                     
      │                             
 ┌────▼─┐                           
 │ SSD  │                           
 └──────┘                           
```
- DPDK: Data Plane Development Kit (kernel bypass for NICs)
- VFS: Virtual File System (linux filesystem abstraction that weka implements)
---
Source: https://www.weka.io/wp-content/uploads/resources/2023/03/weka-architecture-white-paper.pdf
