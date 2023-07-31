Internet protocol
# Internet Protocol
Created: 2023-04-30 20:35
Tags: 
____
#### The IP building blocks

> when we say packet it's layer 3


IP Address
* Layer 3 property
* Can be set automatically or statically
	* [[DHCP]]
* Network and Host portion
* 4 bytes in IPv4 - 32 bits
* a.b.c.d/x 
	* a,b,c,d are integers
	* x is the network bits and remains are host
	* example 192.168.254.0/24
		* the first 24 bits (3 bytes) are network the rest 8 are for host
		* This means we can have 2^24 networks and each network has 2^8 hosts
		* Also called a [[subnet]]
		* The subnet has mask 255.255.255.0
		* subnet mask is used to determine whether an IP is the same subnet

### Default Gateway
* Most networks consists of hosts and a Default Gateway
* Host A can talk to B directly if both are in the same subnte
* Otherwise A sends it ti someone who might know, the gateway
* The gateway has an IP address and each host should know its gateway









_____
##### References
1.

