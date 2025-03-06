Tags:
- [[Spark]]
- [[Distributed Systems]]
---
YARN (Yet Another Resource Negotiator): Cluster management technology

**Architecture**
![[Pasted image 20240810135626.png]]
![[Pasted image 20240810140220.png]]
**Workflow**
- Spark app is submitted to the resource manager
- Spark driver is run in the ApplicationMaster (inside one of the YARN worker nodes)
- ApplicationMaster negotiates resources from ResourceManager
	- each app has 1 application master
	- initial resources for the appmaster is negotiated by the Application Manager in the ResourceManager
	- subsequent resource allocation is by the scheduler, based on scheduling policies
- Spark executors run in the allocated containers on the worker nodes

---
## References
- 