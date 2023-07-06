# Long / Short polling
Created: 2022-05-27 07:33
Tags: 
____
#### Short connection and long connection

```ad-tip
title: Short/Long Connection

__Short Connection__:
	TCP connection will be established for each HTTP request, thich is easry to manage.

__Long Connection__:
	You only need to establish a TCP connection once, Later, HTTP request reuse the same TCP connection, which is difficult to manage.


![[connection-live-tcp.png]]


```

#### HTTP Stream


In this model, client open a connection to the server and wait for message.
The connection is kept open ans server pushes messages synchronously to the client when it receives them.

This model is the most "read-time" one of the three
messages goes to the client as soon as they recived by the server
And because of that, it is not the mos scalable one
Server does not know if sclient is ready to consume new messages and may become busy, slowing down the server



Pushing model is appropriate when you have limited clients/consumer ans messages
for example, a chat application like whatsapp where there are few clients per group or channel, RabbitMQ and Redis pub/sib are some technologies using push model.



#### Short Polling
 __repeatedly send HTTP request, query where the target event is complete__

![[short-polling.png]]
Advantage: 
	1. easy to write
Disadvantage:
	1. waste of bandwidth and server resource

#### Long polling:
__hold the HTTP request(loop or sleep) on the server side, wait until the target time occurs.
keep the request waiting for data arrival or appropriate timeout

![[long-polling.png]]


_____
##### References
1.

