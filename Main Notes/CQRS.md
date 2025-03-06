Tags:
- [[Distributed Systems]]
---
Command Query Responsibility Segregation

**Purpose**: Query data persisted through the [[Event Sourcing]] pattern

**Explanation**
- Define a read-only view DB
- this DB also subscribes to the event store to stay up to date
- application queries this DB
- bottom line: separate reads and updates
	- updates are events sent to the event store
	- reads are queries on the read replica

---
## References
- [[Understanding Distributed Systems]]