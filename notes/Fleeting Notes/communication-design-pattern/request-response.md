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

Os synchronous I/

_____
##### References
1.

