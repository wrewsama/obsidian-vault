Tags:
- [[Computer Networking]]
---
Architectural style, NOT a standard or specification (or protocol like HTTP)

Follows these constraints:

- Uniform interface
	- Interface must uniquely identify each resource
	- Resources should have uniform representations in server response. Consumers should use the representations to modify the state of those resources in the server
	- Each resource representation should carry enough info to describe how to process the message
	- Client should only have initial URI of app and dynamically drive all other resources and interactions by using hyperlinks
- Client-Server
	- separations of concerns
- Stateless
	- each request from client to server must contain all info necessary to understand and complete request
- Cacheable
	- res should implicitly or explicitly label itself as cacheable or non cacheable
- Layered System
	- Allows architecture to be composed of hierarchical layers

---
## References
- 