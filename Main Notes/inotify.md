Tags:
- [[Linux]]
---
## what
- event-driven filesystem monitoring system and API in Linux
- pushes _notifications_ to listeners upon filesystem events
    - e.g. file creation, deletion, modification, etc.

## usage
- through tools like:
- `inotifywait`: listens to directory / file and prints events as they happen
```sh
❯ inotifywait -m -e create,modify test
Setting up watches.
Watches established.
test/ CREATE xdd.txt
test/ CREATE xdd.txt
test/ MODIFY xdd.txt
```
- `inotifywatch`: listens to directory / file, records events, then prints a table summarising them upon exit (manual or via timeout)
---
## References
- https://www.man7.org/linux/man-pages/man7/inotify.7.html
- https://man7.org/linux/man-pages/man1/inotifywait.1.html
- https://linux.die.net/man/1/inotifywatch