Tags:
- [[Databases]]
---
- Uses the MergeTree Storage Engine
- each table has its own directory on disk
- the table is split into parts
    - subdirectories in the table's directory
    - each insert creates a new part
    - parts are merged asynchronously
- in each part, each column has its own `bin` file containing the compressed data for that column
    - these are addressed by offset (because each has the same width)
- each column in each part has its own index file
    - sparse index on the primary key points to each column's data using the offset
    - at query time, this is used to see if a part can be skipped by checking the min and max
---
## References
- https://www.alibabacloud.com/blog/clickhouse-kernel-analysis-storage-structure-and-query-acceleration-of-mergetree_597727#:~:text=1.,column%20is%20compressed%20by%20block
- https://clickhouse.com/docs/merges