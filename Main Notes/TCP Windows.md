Tags:
- [[Computer Networking]]
---
- 2 windows: send and receive
- receive window
    - starts at packet $p$, the latest packet such that all packets with sequence number $\leq{seqnum_p}$ have been received
    - when receiving, 
        - advance window if $p$ has changed, deliver all messages moved out of the window 
        - respond with cumulative ACK ($seqnum_p + 1$) and remaining space in the window (`rwnd`)
- send window
    - starts at packet $p$, the latest packet such that all packets with sequence number $\leq{seqnum_p}$ with have been acknowledged
    - size is dependent on TCP congestion control
    - sends `min(cwnd, rwnd)` un-acked packets

---
## References
- https://www.cs.utexas.edu/~lam/396m/slides/Sliding_window+congestion_control.pdf