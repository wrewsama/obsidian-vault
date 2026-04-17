Tags:
- [[Python]]
---
- folder containing [[Python Module]]s
- optional `__init__.py`
    - runs on import (including imports of nested packages and modules: e.g. `import x.y` or `from x.y import z` will run `x/__init__.py`)
    - also runs first when executing the package or anything in it
    - can be used to "hoist" names in nested subpackages / modules up to the top level of the package
- optional `__main__.py`
    - allows you to run a package directly with `python -m my_package`

---
## References
- https://medium.com/@AlexanderObregon/how-pythons-import-system-works-ff2e410edf6b