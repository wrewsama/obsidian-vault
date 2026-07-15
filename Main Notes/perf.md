Tags:
- [[Linux]]
- [[SRE]]
---
## what
- performance analysis tool in Linux
- provides multiple subcommands including:
    - `perf top`: similar to `top` but for the functions / objects used by a given process
    - `perf stat`: high level summary of stats (e.g. execution time, cache hit rate, branch misses)
    - `perf trace`: replacement for [[strace]] with less overhead
        - poll kernel's counters instead of intercepting every single syscall
---
## References
- https://man7.org/linux/man-pages/man1/perf.1.html
- https://man7.org/linux/man-pages/man1/perf-trace.1.html
- https://man7.org/linux/man-pages/man1/perf-top.1.html
- https://man7.org/linux/man-pages/man1/perf-stat.1.html
- https://fdcservers.net/blog/strace-and-perf-linux-troubleshooting-cheat-sheet