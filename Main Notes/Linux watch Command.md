Tags:
- [[Linux]]
---
- executes a given command at set intervals
```shell
watch [option...] command

# e.g.
watch --interval 0.1 "ls -lh | tail -n2" # run that command every 0.1s
```

useful options:
- `--interval` interval in seconds
- `--differences` flag to highlight diff between successive calls

---
## References
- https://man7.org/linux/man-pages/man1/watch.1.html