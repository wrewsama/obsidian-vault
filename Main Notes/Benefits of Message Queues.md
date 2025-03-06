Tags:
- [[Message Brokers]]
---
- Better performance due to async communication
	- no need to await downstream response
- Fault tolerance
	- 1 consumer going down won't affect other consumers
	- data is persistent so downed consumer can resume where it left off after restarting
- Granular scalability / simplified decoupling
	- removes dependencies between upstream and downstream
	- upstream can just publish to MQ topics
	- downstreams can subscribe to topics they are interested in

---
## References
- 