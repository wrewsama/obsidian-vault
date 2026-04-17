Tags:
- [[Python]]
---
## what
- syntax used for globbing e.g. `Path("/path/to/dir").glob(*[abc]*.json)`

## elements
> note: beside `**`, these only check the globbed directory (`/path/to/dir` in the example above)
- `*`: match 0 or more characters
- `?`: match 1 character
- `[<sequence>]`: match any character in the sequence (e.g. `[0-5]`, `[a-z]`, `[axdfg]`)
- `[!<sequence>]`: match anything not in sequence
- `**` match current directory and all subdirectories (`**/something.py`)

> **weird gotcha**: This is case sensitive on Linux and MacOS, but insensitive on Windows
---
## References
- 