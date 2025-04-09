Tags:
- [[Redis]]
- [[Distributed Systems]]
---
- write-back cache: write data to cache, then data is written to the DB in the background
- spin up a background worker on the client-side (can use libraries e.g. Spring Scheduler)
- `RedisGears`: let the Redis cache sync to the DB itself

---
## References
- https://redis.io/docs/latest/operate/oss_and_stack/stack-with-enterprise/gears-v1/jvm/recipes/write-behind/