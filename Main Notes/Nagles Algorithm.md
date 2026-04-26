Tags:
- [[Computer Networking]]
---
- algorithm to minimise small packets, improving efficiency (less overhead of headers)
- intuition: buffer until enough data has been accumulated (based on the MSS)
- can be disabled with the socket option `TCP_NODELAY`
---
## References
- https://medium.com/@ariyanwaliya/tiny-packets-big-delays-how-nagles-algorithm-works-ef83203c4089
- https://orhanergun.net/nagles-tcp-algorithm