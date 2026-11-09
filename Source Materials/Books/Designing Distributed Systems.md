Tags:
- [[System Design]]
---
## Single Node Patterns
(multiple containers on same host)
- sidecar pattern: augment application container
    - main benefit = reusability of the sidecar across different applications
    - parameterise with environment variables
- ambassadors: basically reverse proxy
- adapters: modify interface of applications
    - useful for standardised monitoring / logging solutions

## Serving Patterns
- replicated, load-balanced servers
    - stateless servers
    - readiness probes (not just healthcheck, need to ensure the servers are ready to serve requests)
    - if requests must be stateful, use _session tracked services_ to route requests from the same session to the same server
- sharded services
    - shard using consistent hashing with appropriate shard key
    - main purpose: increase the size of the stored data beyond the limits of any one node
- scatter/gather
    - fan out work to nodes to be processed in parallel
    - straggler problem: need to wait for all nodes' results => delayed by slow nodes
- functions & event driven processing (FaaS and Serverless)
    - often used for event handling
    - can be used as a decorator on another FaaS or a HTTP endpoint
- master election
    - need consensus protocols (paxos, raft)
    - those algorithms are already implemented by k-v servers (etcd, ZooKeeper, consul)
    - use short TTLs and let the master renew the "lease" as long as it's alive

## Batch Computational Patterns
- work queue system
    - queue manager receives independent work tasks
    - workers consume tasks
    - dynamically scale workers based on processing duration & inter-arrival time
- event-driven batch processing (aka workflow systems)
    - multiple work stages with their own work queues
    - subpatterns for each stage
        - copier: fan out the same task to multiple queues
        - filter
        - sharder: organises input tasks into different queues
        - merger: merge several work queues of the same type
- coordinated batch processing
    - similar to workflows, but with the ability to join outputs together
    - subpatterns for each stage
        - join / barrier: wait for all input tasks to complete, then process
        - reduce: combine multiple input tasks into single output (no need to wait for all previous tasks to complete first)
---
Source: https://www.goodreads.com/book/show/34691716-designing-distributed-systems
