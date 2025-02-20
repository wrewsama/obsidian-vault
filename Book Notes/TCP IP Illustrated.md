## Chapter 2: Internet Address Architecture
classes

| Class | Prefix | Net Number (w prefix) | Host Number | Purpose         |
| ----- | ------ | --------------------- | ----------- | --------------- |
| A     | 0      | 8 bits                | 24 bits     | Unicast/Special |
| B     | 10     | 16 bits               | 16 bits     | Unicast/Special |
| C     | 110    | 24 bits               | 8 bits      | Unicast/Special |

| Class | Prefix | Purpose   |
| ----- | ------ | --------- |
| D     | 1110   | Multicast |
| E     | 1111   | Reserved  |
- Subnetting splits host number into Subnet ID and Host ID
- broadcast address for subnet: all bits in Host field = 1
- Classless Inter-Domain Routing (CIDR): variable length net / subnet numbers, defined by CIDR masks
- Aggregation: group based on prefix to decrease routing table size at each node
- IP address space is allocated by hierarchy of authorities (usually ISPs)

## Chapter 3: Link Layer
Key fields in Ethernet Frame:
- Dest MAC
- Src MAC
- Cyclic Redundancy Check (CRC): basically a checksum
- (if Q-tagged), VLAN ID

VLAN: isolate traffic among hosts; 2 hosts on same switch but different VLANs need a router to communicate

Service Sets:
- basic service set (BSS): Access Point (AP) and its associated stations
- Extended Service Set (ESS): BSS's where the AP's are connected via wired distribution service
- A WLAN formed from multiple BSS's is a service set, identified by a SSID

RTS/CTS (request to send / clear to send):
- sender sends RTS, receive sends CTS, then sender sends data
- prevents simultaneous transmissions to receiver 

Loopback:
- allow clients to communicate with servers on the same computer
- IPv4 addresses starting with 127 are reserved for this