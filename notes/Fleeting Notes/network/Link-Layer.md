Created: 2023-07-26 01:06
Tags: Tags: #network 
____
![[DatalinkLayer.png]]


### Introduction

* The data-link layer is the second layer from the bottom in OSI model(open system interconnection) network architecture model.
* it is responsible for node-to-node delivery of data.
* its major role is to ensure error-free transmission of information.
* DLL is also responsible for encode, decode and organize the outgoing and incoming data.
* This is considered the most complex layer in the OSI model as it hides all the underlying complexities of the hardware from the other above layers.

### sublayers

##### Logic link Control (LLC)
* This sublayer of data link layer deals with multiplexing, the flow of data among applications and other services.
* responsible for providing error messages and acknowledgements as well

##### Media Access Control(MAC)
* Mac sublayer manage the device's interaction, reponsible for addressing frames, and also controls physucal meda access.
* The data link layer receive the information in the from of packets from the network layer, it divides packets intro frames and sends those frames bit-by-bit to the underlying physical layer



#### Link Layer

* The network layer passes the datagram down to link layer, which delivers the datagram to the next node along the route.
* Ethernet, WIFI, an the cable access network's DOCISS protocol
* we'll reger to the link-layer packets as __frames__
