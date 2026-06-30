Tags:
- [[Computer Networking]]
---
## what
- receive buffer: buffer packets until they can be delivered in order
- send buffer: buffer packets till they are acked
## significance in production
- can be checked with `ss`
- buffer exhaustion => stalling writes

---
## References
- https://research.cec.sc.edu/files/cyberinfra/files/Lab%208.pdf (1.1)