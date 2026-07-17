Tags:
- [[Linux]]
---
## what
- `sort`: sorts lines in a file or stdin
    - in-memory for small datasets
    - uses external merge sort for larger datasets
- `uniq`: filters out _adjacent_ repeated lines
    - needs to be used after sort

## useful flags
- `sort`
    - `-n`: sort by number, not lexicographically (e.g. 20 will appear before 100)
    - `-h`: sort by human-readable number (e.g. 6G before 7K)
    - `-u`: does what `uniq` does after sort
    - `-f`: ignore case
- `uniq` (these flags are what make it different from adding the `-u` flag to `sort`)
    - `-c`: prefix each line by the number of occurrences
    - `-d`: print only duplicates (1 of each) 
    - `-i` : ignore case (note: don't forget to use `-f` for the sort as well!)
---
## References
- https://www.lesswrong.com/posts/u9n3hfrjkC8J6WMaX
- https://man7.org/linux/man-pages/man1/sort.1.html