Tags:
- [[Distributed Systems]]
---
Enables services in a distributed system to locate and communicate with each other easily

**Key Parts**
- data plane: made up of sidecar proxies deployed alongside each service instance
- control plane: manages and configures the proxies, provide interface for operators

**Key Processes**
- service registration: register a new instance of the service
	- update service registry
- service discovery: help clients get locations of instances of the desired services
	- dns lookups
	- or service registry (some service like [[#ZooKeeper]] or Consul)
- load balancing
- health checking
- dynamic configuration
- authentication & authorisation: e.g. mTLS auth
- security
	- encryption, access control, and security policy
	
**Architecture**
![[Pasted image 20240810223421.png]]

**Communication Flow**
For intra-mesh communication:
1. service instance sends req to its local sidecar
2. sidecar does various actions to the req (e..g load balancing, security checks)
3. sidecar queries service registry for available destination services and routes to one of their proxies
4. sidecar forwards req to destination proxy 
5. dest sidecar performs more actions and forwards to dest service instance
6. response does the same but backwards

For external services, same thing but through an ingress/egress gateway

**Sidecar redirection**
* iptables rules are used to intercept and redirect traffic within the pod
* set up by an init container
* on ingress from outside and egress from the app, traffic is intercepted and redirected to the sidecar 
![[Pasted image 20240828184941.png]]


---
## References
- 