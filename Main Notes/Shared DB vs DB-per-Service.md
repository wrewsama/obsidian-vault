Tags:
- [[Distributed Systems]]
---
Shared DB pros and cons

Pros:
- straightforward ACID transactions can enforce data consistency
- simple to operate

Cons:
- Development time coupling
- Devs for diff services need to coord schema changes
- Runtime coupling
	- diff services could interfere with each other
	- example: svc A could lock a table, blocking svc B

---
## References
- 