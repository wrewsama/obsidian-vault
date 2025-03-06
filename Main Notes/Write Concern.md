Tags:
- [[Tags/Databases|Databases]]
---
Ensures that writes are replicated to a configured number of nodes (1, n, majority) before being considered successful. If not enough replicas acknowledge, the write is rolled back.

**effects**
* durability: write is persisted even if the primary & multiple secondaries die
* consistency: w=majority and r=linearizable forms a quorum which allows for strong consistency
---
## References
- [[NoSQL Distilled]]