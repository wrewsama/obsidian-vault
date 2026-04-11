Tags:
- [[Databases]]
---
## what
- DB made for storing timestamped data

## how do they work
depends on implementation

## examples
- Prometheus
    - create a timeseries for each combination of labels for each metric
    - new data is written into a _head block_ in memory, as well as a WAL for durability
    - the head block is persistent on disk every 2 hours
    - compression
        - uses delta-of-delta encoding for timestamps
        - [[XOR Floating Point Compression]] for metric values
- TimescaleDB
    - special relational postgres table, known as a hypertable
    - split into chunks, most recent chunk in memory => enable faster writes
    - older chunks converted to columnar format => better compression ratios
- Clickhouse
    - columnar DB with timestamp as primary index
- InfluxDB
    - [[LSM Trees]]
---
## References
- https://www.honeycomb.io/blog/time-series-database
- https://clickhouse.com/resources/engineering/what-is-time-series-database
- https://www.influxdata.com/time-series-database/