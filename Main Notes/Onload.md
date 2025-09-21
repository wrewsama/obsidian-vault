Tags:
- [[Operating Systems]]
- [[Computer Networking]]
---
## What it is
- tool enabling kernel bypass when accessing a network interface card
- consists of
    - userspace library
    - specialised driver

## How it works
- instead of context switching into the kernel's network stack, onload implements the network stack in user space
- intercepts network syscalls and redirects them to the userspace network stack
- userspace network stack communicates directly with the NIC through its specialised driver, `ef_vi`
---
## References
- https://databento.com/microstructure/kernel-bypass