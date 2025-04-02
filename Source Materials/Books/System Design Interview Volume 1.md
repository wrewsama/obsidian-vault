Tags:
- [[System Design]]
---
## Scale From 0 To Millions of Users
- Data Tier
    - Relational vs non-relational: Just pick relational unless you need:
        - super low latency
        - massive amounts of data
        - only unstructured data
    - Replication (master-slave)
        - better read performance (replicas can be read in parallel)
        - higher availability
    - Cache
        - good for frequent reads and infrequent writes
        - improves performance and reduces database load
    - Deploy to multiple data centres (together with web tier)
    - Sharding: split up databases based on a shard key
        - need shard key to evenly distribute data
        - may need denormalisation to avoid expensive joins
        - may need to reshard data if a shard gets too big or if it gets too much traffic (hotspot)
- Web Tier
    - Keep web tier stateless
        - store session data in persistent storage, not in the server
        - allows user requests to be served by any server
        - use a load balancer to route requests between multiple servers
    - Deploy to multiple data centres (together with data tier)
        - servers within a data centre mainly access the data tier within the same data centre (lower latency)
        - use geoDNS to route users to the closest data centre 
    - Split into smaller services
        - allows for independent scaling
    - Others
        - logging
        - metrics (e.g. host level stats like CPU usage, aggregated performance metrics like database access times, or key business metrics)
        - automation (e.g. CI)
- User
    - deliver static content from a CDN

## Back-of-The-Envelope Estimation
- Latency numbers every programmer should know
```
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                          14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                           20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy             3,000   ns        3 us
Send 1K bytes over 1 Gbps network       10,000   ns       10 us
Read 4K randomly from SSD*             150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
Disk seek                           10,000,000   ns   10,000 us   10 ms      20x datacenter roundtrip
Read 1 MB sequentially from disk    20,000,000   ns   20,000 us   20 ms      80x memory, 20X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms
```
- common estimations
    - QPS
    - peak QPS
    - storage size
    - number of servers
- tips
    - write down assumptions
    - label units
    - round and approximate
---
Source: https://www.goodreads.com/book/show/54109255-system-design-interview-an-insider-s-guide?ac=1&from_search=true&qid=a6rdJLb4Zd&rank=1
