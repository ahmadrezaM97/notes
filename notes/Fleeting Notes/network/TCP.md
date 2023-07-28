# TCP
Created: 2023-07-26 01:10
Tags:  #network 
____

#### introduction
* The IP protocol at the network layer is inherently unreliable, it is responsible for delivering packets from one ip address to another without any guarantees for delivery, order, or even completeness of the data in the packet.
* This is where TCP steps in to ensure reliable data transmisssion.

1. __TCP is connection-oriendted. Unlike UDP which sends data from one server to multiple servers, TCP establishes a connection between two specific server.
2. __TCP is reliable. TCP guarantees the delivery of the segments, no matter what the network condition it.
3. __TCP is bitstream-oriented. With TCP, application layer data is segmented


![[tcp-packet.jpg]]

#### Establish a TCP connection

![[3-way-handshake.png]]

#### Clsing a TCP connection
![[4-way-handshake-fin.png]]

_____
##### References
1. https://blog.bytebytego.com/p/everything-you-always-wanted-to-know

