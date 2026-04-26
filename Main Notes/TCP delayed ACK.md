Tags:
- [[Computer Networking]]
---
- wait for some time (usually 200ms) before sending ACK
- if there's a response message sent back to the original sender, let it piggyback the ACK 
- else, just send the ACK after the delay
- minimises overhead of extra headers
---
## References
- https://serverfault.com/questions/834326/questions-about-nagle-vs-delayed-ack