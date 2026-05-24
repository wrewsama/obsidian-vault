Tags:
- [[Agentic Engineering]]
---
## architecture
- raw sources
- wiki (LLM-generated): includes summaries, comparisons, syntheses, etc.
- metadata
    - schema: e.g. `CLAUDE.md`
    - `index.md` link + 1 line summary of everything in the wiki, organised by category
    - `log.md`: similar concept to a WAL in a DB. Keep it in a consistent format to make it easily greppable

## operations
- ingest new source
- query
- lint: quality checks on the state of the wiki e.g. contradictions, stale claims, data gaps

## misc tips and tricks
- version control with git => history, branching, collaboration
- [Obsidan Web Clipper](https://obsidian.md/clipper) to convert webpages into markdown
---
Source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
