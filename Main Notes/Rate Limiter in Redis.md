Tags:
- [[Distributed Systems]]
- [[Redis]]
---
**Naive Implementation**
- key with a request count and expiry time
- if count < limit, increment the key and do the request
- else, reject the request with a 429

**Issue with naive**
- race condition (the classic one)
- MULTI/EXEC can't save us as we can't use intermediate results ([[Multi Exec vs Transactions]]) 

**solution 1: rolling window sorted set**
- keep a sorted set for each bucket, store the epoch for each request
- drop all elements before the window we care about using `ZREMRANGEBYSCORE <bucketname> -inf <upperBound>` where `upperBound` = `curEpoch - interval`
- fetch all elements of the set with `ZRANGE <bucketname> 0 -1`
- `ZADD` the current timestamp
- wrap the above in a multi exec
- allow the action if set size < limit else 429

issue: blocked actions count as actions, so with heavy traffic, 0 requests could get through

**solution 2: lua script**
- move the naive logic to a lua script, which can use intermediate results and execute everything atomically

con: logic outside application, hard to test

---
## References
- 