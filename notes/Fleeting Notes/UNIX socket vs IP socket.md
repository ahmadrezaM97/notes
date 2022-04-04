# UNIX socket vs IP socket 1
Created: 2022-04-09 19:44
Tags:  #unix #linux
____

##### A UNIX
- A UNIX socket(Unix Domain Socket), is an [[inter-process communication]] mechanism that allows bidirectional data exchange between processes running on the seme machine.


- IP sockets ( especially [[TCP protocol]] ) are a mechanism allowing communicatio between processes over the network
	- In some cases, you can use TCP/IP sockets to talk with processes running on the same computer ( by using the [[loopback interface]]) 



>IF you decided communicate with processes on the same host,UNIX socket is the better option.


_____
##### Refrences
1. https://serverfault.com/questions/124517/what-is-the-difference-between-unix-sockets-and-tcp-ip-sockets

