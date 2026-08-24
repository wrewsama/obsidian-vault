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

## Devices
- types of devices
    - block: read/write data in fixed chunks (example of block device: disks)
    - character: read/write character streams of data (e.g. `/dev/null`)
    - pipes: similar to a character stream, but with another process at the opposite end instead of a kernel driver
    - socket: Unix domain sockets for IPC
- Small Computer System Interface (SCSI): protocol for communication with device peripherals (e.g. SATA disks, USB devices)
- `/sys/devices` path: device management for programs
- `/dev` path: device access for user processes
- on new device connection
    - kernel sends notification (called a `uevent`) to `udevd`, a user space process
    - `udevd` load attributes from the `uevent` and takes actions based on its preconfigured rules
        - e.g. creating device file, initialising device, etc.
- `udevadm`: administration tool for `udevd`
    - explore system devices
    - monitor kernel `uevents`
## Disks and Filesystems
- abstraction layers within the disk: OS system calls -> filesystem -> block device interface -> drivers -> hardware storage device
- disk is subdivided into **partitions**
    - the partitions are defined in the **partition table**
    - view the partition table with `sudo parted -l`
- filesystems: a "database" that turns the block device into the hierarchy of subdirectories and files that we're familiar with
    - interact with filesystem mounts with `mount`
    - view filesystems and mount points by reading `/etc/fstab`
    - (**only run on unmounted filesystems !!**) repair a filesystem with `fsck`

## How The Linux Kernel Boots
- BIOS loads and runs boot loader
- boot loader finds kernel image on disk, loads it into memory, and starts it
    - boot loader passes in _kernel parameters_ when starting the kernel. You can view them in `/proc/cmdline`
    - boot loader accesses disk via the BIOS, since the root filesystem hasn't been mounted yet
- kernel initialises devices and their drivers
- kernel mounts root filesystem
- kernel starts `init` (this step is also known as the _user space start_)
- `init` starts the rest of the system processes
- `init` starts a process that lets the user log in

## How User Space Starts
- startup order
    - `init`
    - essential low level services (e.g. `udevd`, `syslogd`)
    - network config
    - mid and high level services (e.g. cron, printing)
    - login prompts, GUIs, and other high level apps
- `systemd`: common implementation of `init`
    - startup process
        - loads config
        - determines boot goal in `default.target`
        - resolves dependency tree of default boot goal
        - activates dependencies and boot goal
        - after startup, can react to system events and activate additional components
    - manages _units_ (services, mounts, targets)
        - configured using _unit files_ in `/usr/lib/systemd/system`
        - can view and manage with `systemctl`
- shutdown process
    - init asks process to shut down cleanly
    - init sends SIGTERM
    - init sends SIGKILL
    - system files get locked
    - non-root filesystems get unmounted
    - root filesystem gets remounted as read-only
    - buffered data gets flushed to filesystem
    - kernel shuts down
- initial RAM filesystem: allows drivers on disk to be loaded on startup when the filesystem isn't mounted yet

## System Config
- most system config files are found in the `/etc` directory
- `syslog`: inside `/etc/rsyslog.conf` or `/etc/rsyslog.d`
    - maps _selector_(where to get logs from) to _action_ (where to send logs to)
- user management
    - `/etc/passwd`: username to user info (e.g. user id, group id, house directory, etc.)
    - `/etc/shadow`: username to encrypted password
- crontab: `/etc/cron`
- `at` lets you schedule 1-time tasks without cron
- user ids are switched using the `setuid` syscall
- Pluggable Authentication Modules (PAM): user-space shared library that handles authentication for a given user
    - config in `/etc/pam.d`
    - config provides rules that are used to determine whether authentication is successful or not
---
Source: https://www.goodreads.com/book/show/514432.How_Linux_Works?ref=nav_sb_ss_1_15
