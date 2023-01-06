# backend protocol
Created: 2023-01-06 20:04
Tags: 
____

Connection Establishment

1. Server Listen on an address:port

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
_____
##### References
1.

