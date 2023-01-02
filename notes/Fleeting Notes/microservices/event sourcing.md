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




_____
##### References
1.

