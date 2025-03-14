Tags:
- [[Operating Systems]]
---
## Writes
- In SSDs, we can only write to empty pages
- We can only empty an entire block (collection of pages) together at once
- To update a page, we write to a new, empty page, then invalidate the old location of the page
- In the background, the SSD's GC will look for blocks with invalid pages, copy the valid pages to other empty spaces, then empty that block
- By doing sequential IO, we can invalidate whole blocks so GC moves fewer pages

## Reads
* Random IO => More operations => More overhead (e.g. logical <> physical memory mapping table lookups)
- Sequential generally has better performance than random, but the difference is less significant than HDDs (<10x vs >100x)
---
## References
- [[Operating Systems Three Easy Pieces]]