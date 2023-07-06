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

client
* Once the connection is established, the client can send requests to the backend.
* the request is really nothing but a series of bytes with  a clear start and end defined by the protocol that is used.
* The client encrypts the request ( if TLS is used on the connection), the compresses body(if request compression is supported) and serialize the data type (JSON/ Protobuff etc) to an on-wire representation. then finally writes the raw byte in network byte order the connection.

server
* Those raw bytes reach the OS kernel from the NIC and go into the connection receive queue managed by the kernel
* packets set there until the  beackend application invoke read() or rcv() syscall which them moves the data from the receive quque to the backend process user space memory.
* Those are just raw bytes that are encrypted and encoded, there is no request here just bytes, for all we know those bytes we read cloud be 10 requests or could be half of a request, we don't know.

**Decrypt**

* now that we have raw bytes in the backend process memory and we know those are encrypted, we invoke the SSL library that our code is linked to (OpenSSL for example) and let it decrypts the content for use so we make sense of it.
* Dectyption is CPU bound operations, it can be done it its own thread or in same thread as  read and accept.

**Parse**
* 




_____
##### References
1 https://medium.com/@hnasr/the-journey-of-a-request-to-the-backend-c3de704de223


