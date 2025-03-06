Tags:
- [[Tags/Databases|Databases]]
---
- alternative form of concurrency control that minimises locking => better performance
- default for PostgreSQL and MySQL InnoDB (though the locks are still available for manual use)
- each statement sees a snapshot of data, regardless of the current state
- reading never blocks writing and writing never blocks reading
---
## References
- https://www.postgresql.org/docs/current/mvcc-intro.html
