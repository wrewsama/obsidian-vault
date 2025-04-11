Tags:
- [[Message Brokers]]
---
> Note: This paper was published in 2011 so some of the info may be outdated. Need to check this.

## Architecture
- **Topic**: stream of messages
- **Broker**: server that stores messages
- **Partitions**: "shards" of a topic, distributed among brokers
## Internals
- Each partition is essentially a write-ahead log
- implemented as a set of segment files
- new messages are appended to the last segment file
- each message is identified by its logical **offset** within the partition
- an in-memory index maps offset to the actual location of the file on disk
- Uses the **Linux sendfile API** to efficiently transfer data from the file to the socket, minimising copies

---
Source: https://notes.stephenholiday.com/Kafka.pdf
