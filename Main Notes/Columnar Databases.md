Tags:
- [[Databases]]
---
## what
- database that stores data by column instead of by row
- used for OLAP (Online Analytical Processing) workloads
    - aggregations of single columns
    - e.g. min and max bid price for a given instrument, over a period of time
## how it works
- columns are kept contiguously on disk, sorted by the primary key, with a sparse index on it (marking the value every 8192 values) for efficiency
- column values for a given "row" are linked based on their position
    - i.e. `col1[i]` corresponds to `col2[i]`

## pros and cons
(+) reduce unnecessary IO on irrelevant columns => faster
(+) more similar values grouped together => better compression
(-) inefficient at OLTP (Online Transaction Processing) workloads like frequent single-row CRUD and ACID transactions
## examples
- ClickHouse
- DuckDB
---
## References
- https://cratedb.com/data/columnar-database
- https://www.flexera.com/blog/finops/clickhouse-architecture/