Tags:
- [[Computer Networking]]
---
- Sender sends to multicast group (identified by IP address and port)
    - IPv4 range 224.0.0.0 - 239.255.255.255
- Receivers
    - bind to the multicast group
    - Join multicast group by setting `sock opt`, specifying which network interface to receive messages from (otherwise will receive on all interfaces, which could lead to duplicates if the host has multiple NICs)
---
Source:
- https://stackoverflow.com/questions/603852/how-do-you-udp-multicast-in-python
