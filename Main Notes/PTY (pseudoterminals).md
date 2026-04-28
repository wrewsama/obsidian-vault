Tags:
- [[Linux]]
- [[Operating Systems]]
---
## what
2 parts with bidirectional communication
    - slave: opened by process that expects to be connected to an actual terminal for I/O (e.g. zsh, Neovim)
    - master: opened by the controlling process (e.g. Terminal, sshd)

## example data flow
- we open `zsh` in `kitty`
- we type `ls` into `kitty` and hit enter
- the command goes from kitty -> master -> slave -> `zsh`
- `zsh` runs the command and writes the output -> slave -> master -> kitty
- kitty displays the result on screen

---
## References
- https://en.wikipedia.org/wiki/Pseudoterminal
- https://man7.org/linux/man-pages/man7/pty.7.html