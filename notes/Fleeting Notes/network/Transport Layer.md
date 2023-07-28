Created: 2023-07-26 23:23
Tags:  #network 
____

![[tcp_udp_headers.webp]]

![[packets.jpg]]

![[packets.jpg]]


* The internet's transport layer transports application-layer message between application endpoints.
* in the internet there are two transport protocols, [[TCP]] and [[UDP]], either of which can transport application-layer message.
* TCP
	* TCP provides a connection-oriented service to its applications.
	* This service includes guaranteed delivery of application-layer messages to the destination and [[flow control]].(sender and receiver speed matching)
	* TCP also breaks long messages into shorter segments and provides a [[congestion-control]] mechanism, so that a source throttles its transmission rate when the network is congested.
* UDP
	* The UDP provide a connection less service to its applications. 
	* This is a no-frills service that provides no reliability, no flow control, and congestion control.
* we'll refer to a transport-layer packet as a __segment__.