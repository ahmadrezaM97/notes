# backend protocol
Created: 2023-01-06 20:04
Tags: 
____

Connection Establishment

![[syn-ack-queues.png]]

During the [TCP]] three-way handshake process, the linux kernel maintains two queues, namely:
1. `SYN` queue
2. `Accept` queue

Process

1. The client side sends `SYN` to the server side to initiate a connection. 
	1. The client side enters the `SYN_SENT` state.
2. After the server side receives the `SYN` request, the Server side enters the `SYN_RECV` state
	1. The kernel will store the connection in the `SYN` queue and reply to the client side with `SYN/ACK`
3. After the client side receives `SYN/ACK`, the client replies `ACK` and enters the `ESTABLISHED` state
4. After the server side receive the `ACK` , the kernel removes the connection from the `SYN` queue and adds it to the `ACPPET` queue
	1. The server side enters the `ESTABLISHED` state.
	2. . A [[file descriptor]]  is created for the connection
5. When the Server side application calls the `accept` function, the connection is take out of the accept queue.



Server Listen on an address:port

```ad-warning
title: 0.0.0.0:400
it is bad practice
if you just listen to this you will listen on all interface
Do not expose for example you database to public internet
```

#TODO search about it

https://medium.com/hackernoon/tcp-three-way-handshake-4161eb8aba32
http://arthurchiao.art/blog/tcp-listen-a-tale-of-two-queues/
https://blog.cloudflare.com/syn-packet-handling-in-the-wild/
https://www.alibabacloud.com/blog/tcp-syn-queue-and-accept-queue-overflow-explained_599203


2. Client connect 
3. kernel does the handshake creating a connection ( not the app!)
	1. kernel creates a socket & two queues `SYN` and `Accept`
	2. client sends a `SYN`
	3. kernel adds to `SYN` queue, replies with `SYN/ACK`
	4. client replies with `ACK`
	5. kernel finish the connection
	6. kernel removes `SYN` from `SYN` queue
	7. kernel adds full connection to Accept queue
	8. `backend` accepts a connection, removes from accept queue
	9. A [[file descriptor]]  is created for the connection
one socket can have many connection, connection is kind of instance of socket
you can have a single process that can handle a thousand connection



problems 
1. backend is not fast enough
2. client who don't `ACK`
3. small backlog
4. 
_____
##### References
1.

