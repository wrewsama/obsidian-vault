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

## Processes and Resource Utilisation
- open files: `lsof`
- system calls: `strace`
- shared library calls: `ltrace`
- threads: `ps m`
- CPU: `top`, `/usr/bin/time`
- adjust niceness: `renice`
- Load averages (average number of processes _running or ready to run_): `uptime`
- memory: `free`
- page faults: `/usr/bin/time`, `ps` with `min_flt,maj_flt` included in the `-o` option
- CPU and memory over time: `vmstat`
- IO: `iostat`, `iotop`
- per-process (cpu, disk, or memory) monitoring _over time_: `pidstat`

## Understanding Your Network and Its Configuration
- network interface management: `ifconfig`
- routing management (ip address prefixes -> gateway ip address): `route`
- ICMP: `ping`, `traceroute`
- DNS hostname <-> ip address lookups: `host`
- hostname overrides: `/etc/hosts`
- DNS config: `resolv.conf`
    - can be a loopback address in order to route to a local caching daemon
    - hostname lookup precedence (e.g. hosts file before dns): `/etc/nsswitch.conf`
- check transport layer connections: `netstat`
- firewall management: `iptables`
- ARP table management: `arp`

## Network Applications And Services
- tools
    - connect to server: `telnet` / `netcat`
    - check users/listeners for a port: `lsof`
    - check received tcp packets: `tcpdump`
    - check available / used ports on a given host: `nmap`
    - check RPC services: `rpcinfo`
- `inetd`  / `xinetd`: superserver used to manage your services' network connections
- unix domain sockets: for IPC over regular IP networking on localhost (also visible using `lsof`)

## Intro to Shell Scripts
- quoting
    - no quotes: shell evaluates substitutions (globs, variables, commands, etc.)
    - single quotes: content is treated as a string as-is
    - double quotes: no globbing or word splitting, substitutions still evaluated
- if/else
```shell
if check_command; then
    command
else
    other_command
fi
```
- notes
    - the `if` branch is taken if the `check_command` exits with an exit code 0
    - can use `&&` and `||` to combine results of check commands
    - the `check_command` is often the `test` or `[` command, e.g. `[ "$1" = hi ]`
    - `test` /`[` provides many operators for checking (e.g. `-gt` for numerical inequalities or `-d` for directories)
- case
```shell
case $1 in
    something)
        command
        ;;
    something_else)
        other_command
        ;;
    *)
        catchall_command
        ;;
esac
```
- for loop (for-each over space delimited values)
```shell
for x in first second third fourth; do
    command
done
```
- while loop (same semantics as the if statement)
```shell
while check_command; do
    command
done
```
- command substitution: pass command's stdout into a command's argument. Use `$()`
- make temporary file: `mktemp`
- heredoc: manually input large amounts of text into a command's stdin: `<<EOF ... EOF`
- quick way to strip directories and extensions: `basename`
- turn stdin into arguments to a command: `xargs`

## Moving Files Across the Network
- simple copies from 1 machine to another
    - `scp`
    - `sftp`
    - `python -m http.server` in the directory you want to share, then access it from `http://HOSTNAME:8000`
    - `rsync`
        - trailing slash on source directory unpacks all its contents into the destination directory
        - use `--exclude` to skip syncing given patterns
- file sharing to/from Windows / Mac machines: Samba client / server
- file sharing with Linux: NFS
    - client: just `mount` the nfs directory at the mount point you want
    - server: can run some server daemons to serve traffic from a normal Linux server, but a NAS (network area storage) device is usually used as the NFS server instead

## User Environments
- common user settings
    - `$PATH`
    - `umask`
    - aliases
- login shells 
    - the shell you use to log in through a terminal, SSH shells included
    - runs `/etc/profile`, then runs the first of `.bash_profile`, `.bash_login`, or `.profile`, depending on what exists
- nonlogin shells
    - additional shell opened after logging in (e.g. when you open the terminal in the Fedora desktop)
    - runs `/etc/bash.bashrc`, then `.bashrc`

## Brief Survey of the Linux Desktop
- X server: communicates with X clients. Takes in requests to draw objects on the screen and sends inputs (e.g. keyboard, mouse)
- X clients
    - window managers: arranges windows on screen and allows users to manipulate them (move, minimise, etc.)
    - widgets (buttons / menus)
    - applications
- Desktop applications communicate through events on the Desktop Bus (D-Bus)
- printing system: CUPS daemon

---
Source: https://www.goodreads.com/book/show/514432.How_Linux_Works?ref=nav_sb_ss_1_15
