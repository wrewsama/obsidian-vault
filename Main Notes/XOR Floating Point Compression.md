Tags:
- [[Databases]]
---
- compression algorithm used for time series metrics that have small changes
- algorithm
    - convert to hex (with some padding to get a fixed number of bits)
    - XOR with previous (this will have many leading / trailing zeroes if changes are small)
    - compress the leading zeroes with the count (e.g. `0b1000` instead of `0b00000000`)
    - keep the nonzero bits as-is
    - trailing zeroes can be derived from the number of nonzero bits and the compressed leading zeroes
---
## References
- https://www.honeycomb.io/blog/time-series-database