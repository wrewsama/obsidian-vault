Tags:
- [[Message Brokers]]
---
How it works:
- event streams are persisted as topics
- those topics are divided into several partitions
- partitions are distributed into different brokers (brokers used to be managed by Zookeeper, now managed by KRaft)
- each partition holds a subset of the records in the topic
- replicas of the partitions are also kept by kafka (on different brokers, obviously) to ensure failure recovery
- the records are assigned a sequential identifier called the offset (unique within the PARTITION)
- producers append data to the topic and the record gets appended to a partition (based on partition key)
- consumers pull messages off a partition in the topic based on their consumer group's offset for that partition. Once a message is consumed, the consumer increments its offset

consumer group: each partition is consumed by exactly 1 consumer in the group


---
## References
- 