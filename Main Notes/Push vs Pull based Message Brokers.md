Tags:
- [[Message Brokers]]
---
## Push based

Producers push messages to consumer

1. producer sends msg to server
2. server immediately delivers it to the consumer

- producer knows if client consumed the message, suits apps that require delivery guarantees

## Pull based

Consumers pull messages from the topic

1. producer sends msg to server
2. consumer pulls any msg from topic

- different consumers can consume messages at a different pace, important for [**backpressure**](https://medium.com/@jayphelps/backpressure-explained-the-flow-of-data-through-software-2350b3e77ce7) handling

---
## References
- 