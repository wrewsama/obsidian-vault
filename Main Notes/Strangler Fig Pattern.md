Tags:
- [[System Design]]
---
## what it is
- pattern to replace legacy system with a newer one without downtime

## how it works
- add facade layer acting as a passthrough to the legacy system
- make clients access the facade instead
- gradually update methods in the facade to call the newer system instead
- once all methods are updated (i.e. facade just becomes a passthrough to the new system)
    - decommission the legacy system
    - make clients access the new system directly

---
## References
- https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig