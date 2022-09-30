# REST
Created: 2022-10-03 02:28
Tags: 
____

#### Client-Server

* The client-server design pattern enforces the __separation of concerns__
* While the client and the server evolve, we have to make sure that the interface/contract between them does not break
* in the REST architectural style, the implementation of the client and the implementation of the server can be done independently without each knowing about the other.

#### Stateless
* Each request from the client to the server must contain all the information necessary to understand and complete the request
* The server cannot take advantage of any previously stored context information on the server -> the client application must entirely keep the session state

#### Cacheable
* the cacheable constraint requires that a response should implicitly or explicitly label itself as cacheable or non-cacheable.

#### Requests
 A request generally consists f
 * An HTTP verb -> kind of operation to perform
 * a Header -> allows the client t pass along information about the request
 * A path to resource
 * an optional message body containing data

#### HTTP Verbs
* Get -> retrieve a specific resource ( by id ) or a collectaion of resources
* POST -> create a new resource 
* PUT -> update a specific resource (by id)
* DELETE -> remove a specific resource by id
*
#### Resource
* the key abstraction of information in REST is a resource
* Any information that we can name can be a resource
* The resource representations are consist of
	* data
	* metadata describing the data
	* hypermedia link that can help the client in transition to the nest desired state

#### Resource Identifiers
* REST uses resource identifiers to identify each resource involved in the interactions between the client and the server component
https://www.codecademy.com/article/what-is-rest
_____
##### References
1.

