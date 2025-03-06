Tags:
- [[Distributed Systems]]
---
**Purpose**
* atomically update database and send messages downstream
* message sent iff DB transaction commits
* ordering is preserved

**Explanation**
- Persist state of business entity as a sequence of state-changing events
- application reconstructs entity's state by replaying the events
- events are persisted in an _event store_
	- API for adding and retrieving events
	- API for subscribing to new events (will receive msg when event is created)
- applications should use [[#Command Query Responsibility Segregation]]

---
## References
- [[Understanding Distributed Systems]]