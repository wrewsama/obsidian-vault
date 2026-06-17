Tags:
- [[Computer Networking]]
---
## what
- special type of [[ARP]] request
- An unsolicited (_gratuitous_) ARP request broadcast by a host for its own IP address
- "I have IP `x` and MAC `y`"

## benefits
- detect IP conflicts (receivers see the request and notice if the broadcasted IP is the same as theirs)
- update other machines' ARP table (useful when the IP changes NIC, which changes its MAC address)
- switch learning

---
## References
- [[TCP IP Illustrated]]
- https://wiki.wireshark.org/Gratuitous_ARP