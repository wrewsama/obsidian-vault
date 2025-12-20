Tags:
- [[Computer Networking]] 
---
HTTP1 : need new tcp conn for each req/res

HTTP1.1:

- has keep-alive mechanism, allowing same TCP conn to be reused for diff reqs
- head of line(HOL) blocking: when 1st req gets delayed, subsequent reqs also have to wait (even with [[HTTP Pipelining]])

HTTP2:

- HTTP streams: multiple streams of objects can be sent on the same TCP conn
	* Essentially, each object is split into frames, the 1st one has the HTTP header and a stream id and subseq parts of the same object have the same stream id
- each stream is independent of each other
- head of line blocking in app layer is solved but issue remains in the transport layer
	- TCP recovering from packet loss will delay other streams bc TCP guarantees in-order delivery so other packets will stay in the buffer till the lost packet is resent and received

HTTP3:
- uses QUIC which is built on UDP
- streams share same conn
- pkt loss on 1 stream doesn't affect others
	- bc QUIC tracks each stream separately (unlike TCP which doesn't know the stream id)
- QUIC uses a Connection ID to allow connections to move between IP addresses and network interfaces quickly and reliably

---
## References
- https://github.com/rmarx/holblocking-blogpost?tab=readme-ov-file#sec_what 
