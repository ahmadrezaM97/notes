# ICMP
Created: 2023-07-30 14:54
Tags: #network 
____

* The Internet Control Message Protocol(ICMP) is a [[Network-Layer]] protocol used by network devices to diagnose network communication issues by performing an error control mechanism. 
* Since IP does not have in inbuilt mechanism for sending error and control messages It depends on ICMP to provide error control.

* **PING** ping is a simple kind of traceroute known as teh echo-request message, it is used to measure the time taken by data to reach destination and return to the sourece, these replies are known as echo-replies messages.
* **Traceroute** tracaroute utility is used to know the route between two deivce connected over the internet.

* ICMP is the primary and important protocol of the IP suite, but ICMP isn't assosiated with any transport layer protocol (TCP | UDP). as it doesn't need to establish a connection with the destination device before sending any message as it is a connectionless protocol

The ICMP packate, the first 32 bits of the packet contains
* Type (8 bit)
	* 0 echo replay
	* 3 destination unreachable
	* 8 echo request
	* 11 time exceeded
* Code
* Checksum
![[icmp.png]]

![[icmp-message-type-field-meanings.jpg]]


![[traceroute.png]]

_____
##### References
1.

