# AGENTS.md — Obsidian Vault

This agent must only read these files — do not create, edit, or delete any notes or configuration.

## Vault structure
- `Main Notes/` — atomic knowledge notes (one concept per file)
- `Source Materials/` — notes on books (`Books/`), videos (`Videos/`), websites (`Websites/`), papers (`Papers/`)
- `Tags/` — empty `.md` files serving as graph nodes (44 tags)
- `Templates/` — note templates (`Atomic Note.md`, `Source Material.md`)
- `Images/` — pasted/attached images

## Note conventions
- **No YAML frontmatter.** Header is plain text: `Tags:` followed by wiki-links, then `---`
- Tags are **wiki-links only** (never `#tag`). Two styles coexist: `[[Tags/Name|Name]]` or just `[[Name]]`
- Main Notes end with `## References` section
- Source Material notes end with `Source:` (URL or plain text)
