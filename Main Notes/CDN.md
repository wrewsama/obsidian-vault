Tags:
- [[Computer Networking]]
---
Serves content to users from nearby edge servers
* reduce latency
* ensure reliability (distribute DDOS attacks)

**Process**
- User sends DNS request for a hostname
- A nearby CDN edge server is returned
	- DNS returns CNAME (canonical name) pointing to domain name of the CDN server
	- DNS requests for the CDN authoritative server, this returns another CNAME pointing to the load balancer
	- DNS request for that load balancer's hostname is sent
	- CDN load balancer picks an edge server (using some load balancing algorithm) and returns its IP
- Edge server checks its CDN cache, if it has the content, return it to the user
- Else, recursively request for it from its parent server, save it in the cache, then return it
	- similar to a read-back cache
	- hierarchy: origin > central > regional > edge

---
## References
- 