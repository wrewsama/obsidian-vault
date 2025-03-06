Tags:
- [[Distributed Systems]]
---
**Issues**
- Clients need to interact with multiple services
- Different clients need different data
- service instances and locations change dynamically (autoscaling)
- services may use non web friendly protocols (e.g. rpc, async pubsub)
- Need to hide the partitioning of the application into services

**Standard API gateway**
![[Pasted image 20240718211032.png]]
- 1 entry point for clients
- different APIs for each client
- API Composition
	- downstreams are queried individually
	- results are joined in memory and processed

**Variant: BFF**
- Backends for frontends
- separate API gateway for each kind of client
	- web
	- mobile
	- 3rd party integrations

---
## References
- 