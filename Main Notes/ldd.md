Tags:
- [[Linux]]
---
- Linux command to **list dynamic dependencies** (`.so` files) of a binary
- shows
    - name of shared library
    - path the library was found
    - memory address where the library gets loaded in

```
$ ldd /bin/ls
        linux-vdso.so.1 (0x00007ffd53bc9000)
        libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007f87bdfea000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f87bddf8000)
        libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007f87bdd61000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f87be046000)
```
---
## References
- 