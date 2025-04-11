Tags:
- [[Message Brokers]]
---
## Architecture
- event streams are persisted as topics
- those topics are divided into several partitions
- partitions are distributed into different brokers (brokers used to be managed by Zookeeper, now managed by KRaft)
- each partition holds a subset of the records in the topic
- replicas of the partitions are also kept by Kafka (on different brokers, obviously) to ensure failure recovery
- the records are assigned a sequential identifier called the offset (unique within the PARTITION)

## Producers
- producers append data to the topic and the record gets appended to a partition (based on partition key)

## Consumers
- consumers are organised into consumer groups by the user
- one of the brokers becomes the Group Coordinator for that consumer group
    - this can be found using the hash of the group id, so all consumers in the group can find out who their coordinator is
    - the consumer group has offsets for each topic & partition stored in the `__consumer_offsets` topic, which is stored in the Group Coordinator
- each consumer is assigned different partition(s) in the topic (each partition has exactly one consumer from the group)
- consume based on the consumer group's offset for that topic & partition, incrementing it and committing the new offset to the Group Coordinator

---
## References
- 