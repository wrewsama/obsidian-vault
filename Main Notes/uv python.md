Tags:
- [[Python]]
---
## What it is
- Python package and project manager
- written in Rust
- significantly faster than `pip`

## Relevant commands
- `uv init`
    - initialise a new project, usually want to use it with the `--package` flag
- `uv add`
    - installs new dependency, automatically updates `pyproject.toml` metadata and lockfile
- `uv remove`
    - removes dependency and updates `pyproject.toml` and lockfile
- `uv sync`
    - sync dependencies from lockfile to the venv
- `uv lock`
    - locks dependencies in venv into the lockfile
    - can also be used to update packages with `--upgrade`
- `uv run`
    - spins up a venv with all the dependencies and runs a script

## Pip interface
- use a venv (create with `uv venv`)
- `uv pip install`
- `uv pip uninstall`
- `uv pip compile pyproject.toml -o requirements.txt`
- `uv pip sync requirements.txt`
---
## References
- https://docs.astral.sh/uv/