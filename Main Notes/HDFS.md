Tags:
- [[Distributed Systems]]
- [[Databases]]
---
* Hadoop Distributed File System
- stores and manages very large datasets across clusters of commodity hardware
- integrates with Hadoop ecosystem
	- MapReduce
	- YARN
	- [[Spark]]

**Architecture**
- files are split into blocks
- stored in multiple DataNodes
	- serve read/write requests
	- perform block creation, deletion, and replication as instructed by the NameNode
	- replication for fault tolerance and availability
- 1 namenode
	- maps blocks to DataNodes
	- regulates client access to files
	- manages file system namespace and metadata

**Read process**
1. client initiates request
2. NameNode is contacted
3. NameNode provides addresses of the closest DataNodes with copies of the desired blocks
4. client connects to the DataNodes (either sequentially or in parallel) and gets the data

---
## References
- 