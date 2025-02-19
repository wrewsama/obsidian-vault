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
