Tags:
- [[Message Brokers]]
---

| Kafka                                                                                                     | RabbitMQ                                                                                  |
| --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Pull-based model                                                                                          | Push-based model                                                                          |
| higher max throughput (1M messages per broker per second)                                                 | less throughput (4K-10K per broker per second)                                            |
| higher latency due to pulling, batching, and accounting overhead (e.g. updating offset) (5-50ms)          | lower latency due to immediate push (1-5ms)                                               |
| is a **durable event stream**: messages are appended and consumers decide where they want to consume from | is a **message queue**: messages are pushed to consumers and get popped upon consumer ack |
(see [[Push vs Pull based Message Brokers]])

---
## References
- https://www.hellointerview.com/blog/kafka-vs-rabbitmq