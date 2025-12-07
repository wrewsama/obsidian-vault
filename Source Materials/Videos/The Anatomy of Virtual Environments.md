Tags:
- [[Python]]
---
## How normal Python installations work
- interpreter binary in `/usr/bin/python*`
    - symlinks to minor version (e.g. `python3` -> `python3.12`)
- dependencies in `usr/lib/python*/site-packages`

## venv
- folder containing
    - `bin`
        - symlinks to interpreter e.g. `python` -> `python3` -> `python3.12` -> `/usr/bin/python3.12`
        - activation scripts
    - `lib/pythonX.Y/site-packages`
        - dependencies (i.e. stuff you `pip install` / `uv add`)
        - `uv` optimises by caching the dependencies centrally, then hardlinking to it in the `venv`'s `site-packages`
    - `pyenv.cfg`
        - metadata
- gets created with `python -m venv {YOUR_VENV_FOLDER_NAME}` or `uv venv`

## activating venv
- save old `$PATH`
- add venv's `/bin` to `$PATH`
- add message to the terminal prompt (usually `(.venv)`)
- this means that the interpreter will be run through the symlink in `.venv/bin`
- the interpreter hence sees its current working directory with reference to the symlink path
- interpreter searches for the `pyenv.cfg` in its parent directory, then gets the `site-packages` relative to that
    - these will be the packages in the venv

---
Source: https://youtu.be/X4HJ5yIAb0c?si=Ri0sZVp2e5_BS7NR
