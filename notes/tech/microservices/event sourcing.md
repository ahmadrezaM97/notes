# event sourcing
Created: 2022-12-30 02:45
Tags: 
____

is a pattern in microservices


### Problem
How to reliably/atomically update the database and send messages/ events?

### Forces
1. [[2PC]] is not an option
2. if the db transaction commits messages must be sent, Conversely, if the database rolls back, the message must not be sent
	1. (message is sent <=> database committed the tranaction)
3. messages must be sent to the message broker in the order they sere sent by the service

#### Solution

Event sourcing persists the state of a business entity such an Order or Customer as a sequence of state-changing events.

Since saving an event is a single operation, it is inherently atomic.

The application reconstructs an entity's current state by replaying the events

Applications persist events in an event store, which is a database of events.
The store has an API for adding and retrieving an entity's events.

The event store also behaves like a message broker.
It provides an API that enables services to subscribe to events.
when a services save an event in the event store it is delivered to all interested subscribes.


![[storingevents.png]]

Benefits 
1. it solves of the key problems in implementing an event-driven architecture and makes it possible to reliably publish events whenever state changes.
2. it provides a 100% reliable `audit log` of the changes made to a business entity


Drawbacks:
	1. it is a different and unfamiliar style of programming and so there is a learning curve
	2. The event store is difficult to query since it requires typical queries to reconstruct the state of the business entities
		1. -> application must use[[CQRS]] to implement queries this in turn means thatapplications must handle eventually consistent data

_____

##### References
1.

