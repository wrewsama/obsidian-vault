Tags:
- [[Tags/Databases|Databases]]
---
Controls what data is read from the cluster.

local: return data from the (primary) instance immediately

available: return data from the (secondary) instance immediately

majority: guarantees data read has been acknowledged by a majority of the replica set members, however, there's no guarantee that it is the latest write (occurs in double-primary situations)

linearizable: returns data that reflects all successful writes acknowledged by a majority before the read begins

---
## References
- [[NoSQL Distilled]]