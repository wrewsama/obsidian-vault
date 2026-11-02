Tags:
- [[SRE]]
- [[Databases]]
---
- transactional outbox pattern
    - backend writes events to an `outbox` table in the same DB transaction as the business operation
    - debezium reads `outbox` changes through the WAL and publishes to kafka
    - prevents missed events when the event publishing fails after the operation was successful (the event is now persisted in the `outbox` table)
- Postgres's **logical replication slot** tracks the next log sequence number needed by debezium
- issue in Postgres 16: logical slot state isn't shared with secondaries, hence failover won't work
- solution: copy logical slot state from primary to secondaries
- solution is natively implemented in Postgres 17+

---
Source: https://shiftmag.dev/how-to-survive-database-failover-debezium-and-postgresql-in-production-11676/
