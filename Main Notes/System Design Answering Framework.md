Tags:
- [[Tags/System Design|System Design]]
---
### 1: Understand Problem & Establish Design Scope
_3-10 minutes_
- ask clarifying questions
	- facts and figures to calculate metrics e.g. QPS, storage requirements
	- features required
	- scaling
	- tech stack / what existing services can we use to simplify the design
### 2: Propose high level design and get buy-in
_10-15 minutes_
* initial blueprint box diagrams
	* clients
	* servers
	* data stores
	* caches
	* MQs
	* CDN
	* etc
* back-of-the-envelope calculations
	- QPS for each function
	- storage requirements
	- etc
* optionally, if design is fairly low-level
	* API endpoints
	* Database Schema

### 3: Design Deep Dive
_10-15 minutes_
- work with the interviewer and dive into desired parts
	- system performance
		- bottlenecks
		- resource estimations
	- go in-depth into components in the high level design

### 4:  Wrap-up
_3-5 minutes_
- answer final questions from interviewer
- dive into additional points
	- recap design
	- error cases
	- operation issues (e.g. monitoring metrics and error logs)
	

---
## References
System Design Interview by Bytebytego