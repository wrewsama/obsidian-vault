Tags:
- [[Distributed Systems]]
---
Choreography: each local transaction publishes a domain event for the next service(s) in line

Orchestration: one orchestrator object tells each service what local transaction to execute

**pros and cons**

| choreo                                | orchestration                   |
| ------------------------------------- | ------------------------------- |
| no single point of failure            | orchestrator object is the SPOF |
| easier to implement                   | harder to implement             |
| challenging to reason about and debug | clearer debugging               |


---
## References
- 