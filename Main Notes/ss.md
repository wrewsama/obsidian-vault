Tags:
- [[Linux]]
---
- command for viewing **socket statistics**
- common examples
    - `ss -l` listening sockets
    - `ss -t` TCP sockets
    - `ss -u` UDP sockets
    - `ss -ltnup 'sport = :8080` processes listening to a tcp or udp socket on port 8080
---
## References
- https://www.baeldung.com/linux/find-process-using-port
- https://man7.org/linux/man-pages/man8/ss.8.html