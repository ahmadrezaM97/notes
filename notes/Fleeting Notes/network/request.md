# request
Created: 2023-07-06 15:05
Tags: 
____

### The Journey of a Request to the Backend

![[reqeust.jpg]]

**Accept**

* Request are often sent on connections(TCP) and connections need to be accepted by the backend.
* When a client connects to a server a 3 way handshake is competed by the server OS kernel and the connection is placed on the listener queue, we call this queue accept queue.
* The backend application is responsible to inovke syscall accept on the listener socket to create a file descriptor which represents the connection.
	* This step can become a bottleneck if the backend is slow in accepting connections.
* Most application dedicate one thread just to accept connections, If a single thread cannot keep up multiple thread can start accepting connections, however this create a bottleneck as threads block each other when accepting on the same socket.
* In this case the [[SO_RESUEPORT]] option can be used to create multiple listeners sockets on the same port with each thread/process owning a socket queue.
https://medium.com/@hnasr/threads-and-connections-in-backend-applications-a225eed3eddb

**Read**





_____
##### References
1 https://medium.com/@hnasr/the-journey-of-a-request-to-the-backend-c3de704de223


