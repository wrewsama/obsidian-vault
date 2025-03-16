Tags:
- [[Computer Networking]]
---
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

## Chapter 4: ARP
- map IPv4 addresses to hardware addresses
- note: IPv6 uses Neighbour Discovery Protocol (inside ICMPv6), NOT ARP
- process
	- ARP request (broadcast) containing desired IPv4 address
	- host sends ARP reply with its MAC address, also learns sender's IPv4 to MAC address
- ARP cache: each host and router caches recent IPv4 <> MAC mappings, default expiry time = 20 minutes
- Gratuitous ARP: host sends ARP request for its own IPv4 address; detect others using the same address

## Chapter 5: IP
- best-effort, connectionless datagram delivery service
- no guarantees that any given packet will reach the destination
- on errors: just drop
- internet checksum: IP's checksum, weaker than CRC
- forwarding
	- if source directly connected to destination, send directly
	- else, send to router and let router deliver to destination
	- forwarding table
		- each entry contains: destination, mask, next hop, and interface
		- find the longest prefix match to the given destination address and take that hop

## Chapter 6: System Config: DHCP and Auto config
- hosts require
    - IP address(es)
    - next hop router
    - DNS server location
- DHCP: Dynamic Host Configuration Protocol. Leases addresses to clients for a defined period of time
- Can work through a relay agent if the client and server are on different networks
- DHCP works for IPv4
- DHCPv6 is for IPv6
- Steps:
    - discover
    - offer
    - request
    - ACK

## Chapter 7: Firewalls and Network Address Translation
- Firewalls
    - purpose: restrict traffic that could harm end systems
    - packet filtering firewalls: drop / forward packets based on configured rules
    - proxy firewalls: act as a gateway, each app-layer service needs to have a proxy handler
- NAT
    - many end hosts share 1 or more globally routable IP

## Chapter 8: Internet Control Message Protocol
- Part of the IP layer
- Used to provide diagnostics and control info through error and control messages
- some ICMP message types
    - Echo request/reply
    - Router advertisement
    - Router solicitation
    - Destination Unreachable
    - Source Quench
    - Redirect
    - Time Exceeded
    - Parameter Problem

## Chapter 9: Broadcasting and Local Multicasting (IGMP and MLD)
types of IP addresses :
- unicast: send to someone
- broadcast: send to everyone
- multicast: send to group of interested receivers
- anycast: send to anyone

---
Source: https://www.goodreads.com/book/show/505560.TCP_IP_Illustrated_Vol_1