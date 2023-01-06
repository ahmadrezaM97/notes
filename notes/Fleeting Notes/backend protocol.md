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

### `ss` command
```basj 
# -n does not resolve the service name
# -t only show tcp sockets
# -l display LISTEN-state sockets

$ ss -nlt
State  Recv-Q Send-Q Local Address:Port  Peer Address:PortProcess
LISTEN 0      4096      127.0.0.11:33785      0.0.0.0:*
LISTEN 0      2                  *:8080             *:*


$ ss -nt
State      Recv-Q Send-Q    Local Address:Port         Peer Address:Port
ESTAB      0      0         [::ffff:33.9.95.134]:80                   [::ffff:33.51.103.59]:47452
ESTAB      0      536       [::ffff:33.9.95.134]:80                  [::ffff:33.43.108.144]:37656
ESTAB      0      0         [::ffff:33.9.95.134]:80                   [::ffff:33.51.103.59]:38130
ESTAB      0      536       [::ffff:33.9.95.134]:80                   [::ffff:33.51.103.59]:38280
ESTAB      0      0         [::ffff:33.9.95.134]:80    

```

For sockets in LISTEN states

* `Recv-Q`: The size of the current accept queue
	* which means the TCP handshake has been completed and are waiting for the application `accept` TCP connection
* `Send-Q`: The maximum length of the accept queues


For sockets in non-LISTEN state
* `Recv-Q`: the number of bytes received but not read by application
* `Send-Q` the number of bytes sent but not acknowledged

### netstat

Run the `netstat -s` command to view the overflow status of `TCP SYN` queue and accepet queue

```bash
$ netstat -s | grep -i "listen"
     4 times the listen queue of a socket overflowed
    4 SYNs to LISTEN sockets dropped
```

the values indicating how many `TCP` sockets links are discarded because the accept and `SYN` queue are full

* 4 time the listen queues of a socket overflowing 
	* -> there are 4 accept queues overflow
*  4 `SYNs` to LISTEN sockets dropped
	* -> 4 `SYN` queues overflow

```ad-note
title: when troubleshooting online issues
if the relevant value has been rising for a while, this means that the SYN queue and accept queue are overflowing
```


#### maximum length control of Accept queue

The maximum length of a TCP queue is controlled by `min(somaxconn, backlog)`

	1. somaxconn is kernel parameter for Linux and is specified by
	 /proc/sys/net/core/somaxconn`
	 2. A backlog is one of the TCP protocol's function paramets, which is the size of the int `listen(int sockdf, int backlog)` function's backslog

```ad-warning
title: in the golang 
backlog paramets of listen function use the value from the 
	 /proc/sys/net/core/somaxconn`
```



#### Slow application 
https://blog.cloudflare.com/syn-packet-handling-in-the-wild/


what happens if the application can't keep up with calling `accept()` fast enough?

This is when the magic happens! when the accept queue gets full then

1. inbound `SYN` packets to the `SYN` queue are dropped.
2. inbound `ACK` packets to the `SYN` queue are dropped
3. the `TcpExtListenOverflows` / `LINUX_MIB_LISTENOVERFLOWS` counter is incremented
4. the `TcpExtListenDrops / LINUX_MIB_LISTENDROPS`  counter is incremented


### Spreading the accept() load
https://blog.cloudflare.com/the-sad-state-of-linux-socket-balancing/
https://www.youtube.com/watch?v=7mTH9AHVFvw

#TODO 
experiment
https://www.alibabacloud.com/blog/tcp-syn-queue-and-accept-queue-overflow-explained_599203

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

