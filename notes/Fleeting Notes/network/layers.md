# layers
Created: 2023-07-19 00:45
Tags: 
____

![[net-model.png]]


![[net-model-2.png]]


#### [[Application Layer]]

* The application layer is where network applications and their application-layer protocols reside.
* The internet's application layer includes many protocols, such as the [[HTTP]] protocol, [[SMTP]] and [[FTP]] and [[DNS]].
* we'll refer to this packet of information at the application layer as a __message__. 

#### Transport Layer

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

#### Network Layer

* The internet's network layer is responsible for moving network-layer packets known as __datagrams__ from one host to another.
* The transport-layer protocol(TCP/ UDP) in a source host passes a transport-layer segment and destination address to the netwrok layer

#### Link Layer

* The network layer passes the datagram down to link layer, which delivers the datagram to the next node along the route.
* Ethernet, WIFI, an the cable access network's DOCISS protocol
* we'll reger to the link-layer packets as __frames__

#### physical layer
* whlie the job of the link layer is move entire frames from one network element to an adjacent network element, the job of physical layer is to move the individual bits within the frame from one node to the next.
_____
##### References
1.

