Tags:
- [[Computer Networking]]
---
## problem
- Normally, TCP uses a cumulative ACK: acknowledge only the latest packet p such that every packet <= p was also received (see [[TCP Windows]])
- Hence, if the sender sends `[a, b]` and only packet `a` is dropped, the destination will keep sending ACKs for `a-1` only, forcing retransmission of the entire window, even if the others > `a` were actually received

## solution
- SACK (selective acknowledgement) allows receiver to tell sender the out-of-order packets it received
- allows sender to not have to send things it knows the receiver already received
---
## References
- https://lwn.net/Articles/791409/
- https://www.geeksforgeeks.org/computer-networks/selective-acknowledgments-sack-in-tcp/