Tags:
- [[Computer Networking]]
---
- address resolution protocol
	- Finds the MAC address associated to an IP address
	- stores these 'key-values' in an ARP table
- If sending outside subnet:
	- send to router's MAC address
- If within subnet
	- if dest IP in arp table, send directly
	- if not, broadcast query pkt to FF-FF-FF-FF-FF-FF, dest will reply with its mac address and that will get cached till TTL expires

---
## References
- [[TCP IP Illustrated]]