Tags:
- [[Tags/Databases|Databases]]
---
Tuples split up into pages in heap files (unordered collection)

location of the pages in the files is tracked by special pages (page directory)

In each page:

- header with metadata (e.g. size, checksum, transaction visibility)
- slot array that maps slots to tuples' starting position offsets

For each tuple:

- has unique record identifier (usually pageid + offset/slot)
- header with metadata (e.g. visibility info)
- attribute data (just a string of bytes)
---
## References
- 