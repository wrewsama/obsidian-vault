Tags:
- [[System Design]]
---
## Proximity Service
- get businesses and info based on location
- geohash
    - recursively split map into 4 parts and assign a 2-bit number to each quadrant
    - the `ith` pair of bits in the geohash refers to which of the 4 quadrants at level `i` the location belongs to
    - geographically closer locations have numerically closer geohashes => we can make an index on this
- business data is stored in business DB (optional cache) and accessed through the business service
- business data is synced with geohash Redis server
- search requests are routed to the Location-Based Service, which calculates the desired geohashes and searches the geohash redis cluster for businesses

## Nearby Friends
- clients use a websocket connection with the load balancer to update location
- load balancer forwards to websocket servers
- sender's websocket servers:
    - send location to its own Redis pub/sub channel
- each user's websocket connection handler subscribes to all their friends' channels
- receiver websocket servers:
    - compute distance from sender's location
    - return to user

## Map
- navigation service
    - based on Dijkstra's, but running it on the entire world map will be inefficient
    - split into sets of routing tiles with different resolutions (e.g. 1 set with big tile / low resolution, 1 set with small tiles and high resolution)
    - run Dijkstra's on those tiles
- location service: track user's location
    - batch location updates
    - use database that supports high write volume and scalability (e.g. Cassandra)
- map rendering
    - serve map tile images from CDN
    - compress the images

## Distributed Message Queue
- Use Zookeeper to handle
    - metadata (info about the topics e.g. number of partitions)
    - state storage (consumer-partition mapping, consumer offsets)
    - coordination (rebalancing, leader election)
- ensure durability with replication
    - configurable ACK setting to balance performance and durability
- producers
    - buffer messages to send them in batches: minimise network costs
    - handles routing to an appropriate broker's partition
- consumers
    - joins a consumer group by sending a message to the group's coordinator
    - group's coordinator tells it which partition to consume from
    - fetch messages from the partition and commit the incremented offset

---
Source: https://www.goodreads.com/book/show/60631342-system-design-interview-an-insider-s-guide?ac=1&from_search=true&qid=rdZCpXSMbI&rank=2
