Tags:
- [[Distributed Systems]]
---
Server for distributed coordination of cloud applications. Critically, it provides a total order over updates, allowing it to be used for coordination

**Use Cases**
- Naming nodes in a cluster
- config management for nodes
- leader election
- locking and synchronisation

**Architecture**
* many servers, each with their own in-memory database
* 1 leader server
* clients connect to different servers
* write requests are all forwarded from the followers to the leader
* leader uses atomic broadcasts to send message proposals to followers
* messaging layer also replaces leaders on failures and syncs followers with leaders

---
## References
- 