Tags:
- [[Linux]]
---
## The Big Picture
- treat the system as 3 layers of abstraction
    - User processes
    - Linux Kernel
    - Hardware

## Basic Commands & Directory Hierarchy
- (skipped the basic commands)
- command line shortcuts
    - ctrl B: move cursor back
    - ctrl F: move cursor forward
    - ctrl P: view previous command
    - ctrl N: view next command
    - ctrl A: move cursor to beginning
    - ctrl E: move cursor to end
    - ctrl W: delete preceding word
    - ctrl U: delete from beginning to cursor
    - ctrl K: delete from cursor to end 
    - ctrl Y: paste erased text
- directory structure
    - /bin: executables
    - /sbin: system executables
    - /dev: device files
    - /etc: system config
    - /home: personal user directories
    - /lib: shared libraries
    - /proc: system statistics
    - /sys: device and system interface, similar to /proc
    - /tmp: temporary storage, gets cleaned up periodically or when machine reboots
    - /var: runtime information (e.g. caches, system logging)
    - /boot: bootloader files
    - /media: mount point for removable media e.g. flash drives
    - /opt: 3rd party software
    - /usr
        - /include: C compiler header files
        - /info: GNU `info` manuals
        - /local: for installing custom software (outside the usual package manager)
        - /man: `man` manual pages
        - /share: files that should work on other unix machines with no functionality loss (no longer very relevant)

---
Source: https://www.goodreads.com/book/show/514432.How_Linux_Works?ref=nav_sb_ss_1_15
