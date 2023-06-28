# network course
Created: 2023-06-28 09:24
Tags: 
____

Media Access Control address (MAC address)
packet -> layer 3 ( data, destination IP, source IP)

IP
* Layer 3 property
* can be set automatically or statically
* network and host portion
* 4 bytes in IPv4 - 32 bits

network vs host
* a.b.c.d/x(a, b, c and d are integers) x is the network bits and remains are host

default gateway
* most networks consists of hosts and a default gateway
* when hos A want to talk to B Directly if both are int same subnet
* Otherwise A sends it to someone who might know, the gateway
* The Gateway has an IP address and each host should know its gateway

ICMP
* internet control message protocol
* layer 3(there are not ports in layer 3)
* Designed for informational messages
	* Host unreachable, port unreachable, fragmentation needed
	* Packet expired (infinite loop in routes)
* Use up directly
* Ping and traceroute use it
* Doesn't require listeners or ports to be opened.
* some firewall block ICMP for security reasons
* That is why PING might not work in those cases
* Disabling ICMP also can cause real damage with connection establishment
	* fragmentation needed
* 
_____
##### References
1.

