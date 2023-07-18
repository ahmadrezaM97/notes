# layers
Created: 2023-07-19 00:45
Tags: 
____

![[net-model.png]]


![[net-model-2.png]]


#### Application Layer

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


_____
##### References
1.

