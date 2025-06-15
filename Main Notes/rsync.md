Tags:
- [[Linux]]
---
## Overview
- syncs entire directory trees between local and remote file systems
- transfers only deltas
- `rsync -a --delete /source/dir user@remote:/dest/dir` and vice versa

## How it works
- Sender (source) recursively scans file tree and generates a file list containing the names and metadata of each file and sends it to the receiver
- Receiver (destination) takes the list and, for each file on the list that exists on the destination, compute rolling checksums and send them back to the sender
- Sender compares checksums against the checksums on its own version of the files to identify deltas
- Sender sends deltas for updated files and sends the full files for files that don't exist on the receiver
- Receiver applies deltas to update the files

---
## References
- 