Tags:
- [[Python]]
---
## basic definitions
- [[Python Module]]
- [[Python Packages]]

## import process
- check module cache in `sys.modules`
    - if inside, just use that
- search for the module in the directories found in `sys.path`
    - e.g. current working directory, `PYTHONPATH`, `site-packages`, etc.
- insert empty module object into `sys.modules`
    - prevent concurrent imports from also executing the module
- execute the code ([[Python Execution]]) in the module and populate the empty module object with the names and values

> **Note**: The entry point (the module you execute `python -m` on) is added to `sys.modules` as `__main__`. If you execute `import <that entrypoint module>`, that entry point will be executed again
---
## References
- https://medium.com/@AlexanderObregon/how-pythons-import-system-works-ff2e410edf6b