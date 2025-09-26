Tags:
- [[Python]]
---
## What it is
- Python package and project manager
- written in Rust
- significantly faster than `pip`

## Relevant commands
- `uv init`
- `uv add`
- `uv remove`
- `uv sync`
- `uv run`

## Pip interface
- use a venv (create with `uv venv`)
- `uv pip install`
- `uv pip uninstall`
- `uv pip compile pyproject.toml -o requirements.txt`
- `uv pip sync requirements.txt`
---
## References
- https://docs.astral.sh/uv/