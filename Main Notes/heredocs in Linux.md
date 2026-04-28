Tags:
- [[Linux]]
---
## what
- way to put a multiple lines into the stdin of a bash command 
- useful for passing multiple lines into a CLI command (e.g. `cat` multi-line string or `ssh` multiple commands)
- the delimiter can be put in quotation marks to prevent variable (e.g. `$HOME`) expansion

## example
```shell
❯ cat << MY_DELIMITER
heredoc> lol  
heredoc> hi
heredoc> MY_DELIMITER
lol
hi
```

---
## References
- https://anju-chaurasiya2012.medium.com/heredocs-in-bash-scripting-5f4d8b7589d1