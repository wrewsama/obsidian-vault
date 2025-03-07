Tags:
- [[Operating Systems]]
---
1. BIOS/UEFI boots up
	1. Initialises hardware
	2. UEFI is essentially a better BIOS
		1. faster
		2. secure boot
2. BIOS/UEFI runs Power On Self Test (POST)
	1. ensures hardware is working before fully turning everything on
	2. throw error if any
3. find and load boot loader software
	1. usually GRUB2
	2. locates OS kernel on disk
	3. load kernel into mem
	4. run kernel code
	5. hands over control to kernel
4. kernel takes over computer's resources
5. loads hardware drivers and modules
6. mounts the root filesystem
7. run init process (usually systemd)
	1. initialises everything that needs to launch behind the scenes when starting up linux

---
## References
- 