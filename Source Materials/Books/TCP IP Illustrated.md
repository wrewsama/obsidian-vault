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

## Chapter 10: UDP and IP Fragmentation
- UDP header contains:
    - source port
    - destination port
    - length
    - checksum
        - computed over UDP packet with an additional pseudo-header containing fields e.g. source and destination IP addresses, taken from the IPv4 header
- IP fragmentation
    - To keep IP datagram size <= Max Transmission Unit
    - Path MTU can be discovered using UDP (known as PMTUD - path MTU discovery mechanism)

## Chapter 11: DNS
- Domain Name System
- Maps between host names and IP addresses
- Applications access DNS through a resolver
- DNS name space is case insensitive and partitioned hierarchically
    - The owner of a portion of the namespace (aka a _zone_) needs to have at least 2 DNS servers to hold info for that portion
- Name servers besides some of the root and TLD will cache information up to a given TTL
- common DNS record types
    - A (address): name to IPv4 address
    - AAAA (address): name to IPv6 address
    - NS (name server): zone to authoritative name server's name
    - CNAME (canonical name): name to its canonical name
    - SOA (start of authority): authoritative info for the zone
    - PTR (pointer): IPv4 / IPv6 address to canonical name

## Chapter 12: TCP
- sliding window: sender can only send within the window
- window can be used for flow control
- retransmission timeout is based on a sample mean of RTTs
- TCP header contains:
    - source port
    - dest port
    - sequence number
    - acknowledgement number
    - checksum
    - window size
    - etc.

## Chapter 13: TCP Connection Management
- TCP connection: 4-tuple with 2 endpoints: (IP address, port number)
- Setup:
    - client sends SYN
    - server sends SYN + ACK
    - client sends ACK
- Teardown:
    - client sends FIN
    - server sends ACK
    - server sends FIN + ACK
    - client sends ACK
- Path MTU discovery:
    - set Send Max Segment Size (SMSS) = `min(outgoing_mtu, dest_mss)`
    - if a Packet Too Big ICMP error is received, decrease SMSS
    - if no decreases for some time, increase SMSS
- TCP RST (reset segment): when a segment that's invalid for the current connection is received
- SYN flood: DoS attack where clients spam SYN segments at a server, wasting connection resources and preventing legitimate requests from being served 
    - addressed using SYN cookies

## Chapter 14: TCP Timeout and Retransmission
- timeout-based retransmission
    - retransmit whenever timeout is reached
    - use binary exponential backoff for retransmission interval until ACK is received
    - measure RTT and update RTT estimate (mean of sampled RTTs)
- fast retransmit
    - retransmit when >= `dupthresh` duplicate ACKs are received
    - Selective ACK (SACK): server sends info about which packets are missing => client only needs to resend the missing ones, not all sent packets with sequence number > the duplicate ACK number => better performance

## Chapter 15: TCP Data Flow and Window Management
- Interactive data (e.g. `ssh`) transmits small segments (< SMSS)
- addressed by:
    - delayed acknowledgements: wait a bit to send some data along with the ACK
    - Nagle algorithm: sender buffers the small packets until all outstanding data has been ACKed, then send everything in 1 larger packet
- flow control is implemented in TCP via a window advertisement on each ACK

## Chapter 16: TCP Congestion Control
- Congestion: when router needs to drop packets due to overwhelming traffic
- Congestions detection
- slowing down sender
    - set the usable window to be the minimum of the receiver's advertised receive window and the congestion window (estimate of the network's capacity)
- Slow Start: on new TCP connection or when loss is detected (through retransmission timeout). Start with a small window and grow exponentially if no loss. Halve if there's loss. Switch to congestion avoidance once threshold reached.
- Congestion Avoidance: grow window linearly if no loss
- Delay-based congestion control: Instead of slowing down after detecting loss, slow down when RTT is too high

---
Source: https://www.goodreads.com/book/show/505560.TCP_IP_Illustrated_Vol_1