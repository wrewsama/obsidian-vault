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

---
Source: https://www.goodreads.com/book/show/60631342-system-design-interview-an-insider-s-guide?ac=1&from_search=true&qid=rdZCpXSMbI&rank=2
