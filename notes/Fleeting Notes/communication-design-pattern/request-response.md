# request-response
Created: 2023-03-20 19:48
Tags: 
____

#### Request-Response

* Client send a Request
* Server parses The Request
* Server processes the Request
* Server sends a Response
* Client parses The Response and consumer

```ad-note
the cost of parsing request is no cheap
```

* Example
	* [[HTTP]], [[DNS]], [[SSH]]
	* [[RPC]] (remote procedure call)
	* SQL and Database Protocols
	* APIs([[REST]]/ [[SOAP]] / [[GraphQL]])

Anatomy of A Request / Response
* A request structure is defined by both client and server
* Request has a boundary
* Defined by a __protocol__ and __message format__
* Same for the response

* Dosen't work anywhere
	* notification service
	* Chatting application
	* Very long request


### Synchronous vs Asynchronous

> can I do work while waiting

Synchronous I/O

* Caller sends a request and blocks
* Caller cannot execute any code meanwhile
* Receiver responds, Caller unblocks
* Caller and Receiver are in "sync"

OS synchronous I/O
* program asks OS to read from risk.
* Program main thread is taken off of the CPU.
* Read completes, program can resume execution.

Asynchronous I/O
* Caller sends a request
* Caller can work until it gets a response
* Caller either
	* checks if the response is ready ([[epoll]])
	* Receiver calls back when it's done ([[io_uring]])
	* Spins up a new thread that blocks
*  Caller and receiver are not necessary in sync

Synchronous vs Asynchronous in Request Response
* Synchronicity is a client property
* Most modern client libraries are asynchronous
* E.g. Client send an HTTP request and do work

Asynchronous workload is everywhere
* Asynchronous Programming ( promises/ futures)
* Asynchronous backend processing
* Asynchronous commits in postgres 
* Asynchronous I/O in linux ( epoll, io_uring)
* Asynchronous replication
* Asynchronous OS fsync( fs cache)

#### Push

```ad-note
if you want as soon as possible use PUSH.
```

* Client wants real time notification from backend
	* A user just logged in
	* A message is just received

What is push
* Client connects to a server
* Server sends data to the client
* Client doesn't have to request anything
* Protocol must be bidirectional
* Used by [[RabbitMQ]]
	* `Rabbitmq` push queue's messages to consumers
	* kaka use long-polling

* Push Pros
	* Real-time
	* 
* Push Cons
	* Client must be online
	* __Clients might bot be able to handle__
	* Requires a bidirectional protocol
	* Polling is preferred for light client

#### Polling

##### Short Polling

> Request is taking a while, I'll check with you later.

* Client sends a request
* Server responds immediately with a handle
* Server continues to process the request
* Client uses that handle to check for status
* Multiple "short" request response as polls

[[Long-or-Short polling]]

![[short-polling2.png]]

* Short polling Pros
	* Simple
	* Good for long running requests
	* Client can disconnect (save request id/ task id)
* Short polling Cons
	*  Too chatty
		* lots of false
	* Network bandwidth
	* Wasted backend resources


#### Long Polling

> request is taking long, I'll check with you later but talk to me only when it's  ready

* Client sends a request
* Server responds immediately with a handle
* Server continues to process the request
* Client uses that handle to check for status
* __Server Does not reply until it has the response__
* So we got a handle, we can disconnect a client

```ad-note
kafka use this
```

![[long-polling-2.png]]



* Long polling pros
	* Less chary and backend friendly
	* Client can still disconnect
* Log polling pros
	* Nor real time


#### Server Sent Events

> one request, a very very long response> 

* A response has start and end
* Client send a request
* Server sends logical events as part of response
* Server never writes the end of the response
* It is still a request but an unending response
* Client parses the streams data looking for events

![[sse.png]]


* SSE Pros
	* Real time
	* Compatible with request/response
* SSE cons
	* Client must be online
	* Client might bot be able to handle (like what happends in push)
	* Polling is preferred for light clients
	* HTTP/1.1 (problem) (6 connections)



### Publish Subscribe

> one publisher many readers


### multiplexin
### Stateful vs stateless
### Sidecar pattern

_____
##### References
1.

