Tags:
- [[Computer Networking]]
---
- Automatic probe request sent out to keep a TCP connection alive
- Destination should ACK the request
- useful for
    - checking for dead peers
    - preventing disconnections due to inactivity (useful for NAT)
- important defaults for Linux
    - Idle time before TCP keepalive probes are sent: 2 hours
    - time before re-probe if the previous wasn't acknowledged: 75 seconds
    - number of probes before giving up: 9

---
## References
- https://tldp.org/HOWTO/TCP-Keepalive-HOWTO/overview.html