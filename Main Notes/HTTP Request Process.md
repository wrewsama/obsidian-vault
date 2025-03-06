Tags:
- [[Computer Networking]]
---
1. (when client first connects to network) DHCP(dynamic host config protocol) request from client
	1. encapsulated in UDP, IP, then 802.3 ethernet (or 802.11 wifi)
	2. Ethernet frame broadcast 
	3. switch learns interface of the client
	4. DHCP server formulates DHCP ACK with client's IP, IP of first hop router, name & IP of DNS server
	5. forwarded back to client directly (thanks to switch learning)
2. client does ARP(address resolution protocol) query broadcast
	6. router replies with ARP reply with MAC address of router interface
3. client sends DNS query
	1. encapsulated in UDP, encapsulated in IP, encapsulated in ethernet
	2. forwarded by switch to first hop router
	3. IP datagram forwarded through some protocol to local DNS server
	4. (if answer not in cache) local DNS server queries root DNS server to get TLD server IP, which returns the relevant authoritative server ip, which gives the IP address of the URL, the local DNS server then replies to client with this IP address
4. client finally sends HTTP request
	1. open TCP socket to web server
	2. establish TCP connection with 3 way handshake
	3. send HTTP request into TCP socket
	4. IP datagram containing req gets routed to server
	5. webserver responds with HTTP response

---
## References
- 