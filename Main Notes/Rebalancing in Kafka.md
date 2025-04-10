Tags:
- [[Message Brokers]]
---
Reassign topic partitions to consumers in a consumer group

**triggers**
- new consumer joining
- new partition added
- consumer leaving or dying (identified by lack of heartbeat)

**Process**
- notify all consumers in the affected consumer group
- consumers respond with ready message
- group coordinator (one of the brokers) assigns partitions to consumers

can be eager (stop all consumers) or cooperative (reassign only a subset of partitions)

---
## References
- 