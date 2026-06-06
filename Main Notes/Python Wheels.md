Tags:
- [[Python]]
---
- prebuilt distribution for libraries
- implemented as `.whl` files that are essentially ZIP archives with a special naming convention
    - ZIP archive contains the prebuilt binaries for a given environment
    - also includes all required metadata for the library
    - the environment is in the wheel file name, the installer (e.g. `pip`) will look for and install the correct one
- saves installation time compared to source distributions, which require building (compiling C/C++ and generating metadata)
---
## References
- https://realpython.com/python-wheels/