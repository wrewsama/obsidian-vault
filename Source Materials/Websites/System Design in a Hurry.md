Tags:
- [[System Design]]
---
## Delivery Framework
- Requirements
    - Functional: "Users should be able to X"
    - Non-Functional: "The system should be Y"
- (optional) capacity estimation (only if there are relevant back-of-the-envelope estimations that would affect the design)
- core entities
- API
    - protocol
    - endpoints
- (optional) Data flow (only if there are lengthy flows of data)
- High level design
    - diagram for each API endpoint
    - diagram showing how the different components
    - focus on a simple design that covers the functional requirements
- Deep dives (might be interviewer-led)
    - adjust to meet non-functional requirements
    - address edge cases
    - in general, improving on the high-level design

## Core Concepts Overview
- Network protocols: HTTP / SSE / websockets / gRPC
- Load balancers: layer 7 vs layer 4
- API design
- Data modelling: Relational vs Non-relational, normalised vs denormalised
- Database indexing
- Caching: pattern (e.g. cache-aside), invalidation strategy, failure handling
- Sharding: choice of shard key, ensuring transactions aren't cross-shard
- Consistent hashing
- PACELC theorem
- key latency and concurrency numbers for common components

## Key Technologies Overview
- Databases
    - relational
    - non-relational
- blob storage
- search optimised database (inverted index)
- API gateway
- Load balancer
- Queue
- Streams
- Distributed Lock
- Distributed Cache
- CDN

---
Source: https://www.hellointerview.com/learn/system-design/in-a-hurry/introduction
