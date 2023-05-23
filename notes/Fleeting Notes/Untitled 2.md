

https://refactoring.guru/design-patterns/chain-of-responsibility/go/example



### Aggreagate

https://www.youtube.com/watch?v=zlFqjD2LKlE

* An Aggregate is a __cluster of associated object__ that we __treat as a unit__ for the purpose of __data changes__.
* Each aggregate has a __root__ and a __boundary__.
	* The root is a single, specific Entity contained in the Aggregate.
	* The aggregate boundary is consistency boundary
		* everything within this boundary should be immediately consistent whenever data changes occur.
		* Preferably in a atomic operation, i.e an ACID transaction.
* All data changes are handled through the Aggregate ROOT
	* but root can delegate capabilities to other objects if needed.
* Invariants are rules that need to be satisfied at all times, they are an integral part of the consistency guarantees.
* Outer World
	* Other parts of the model are allowed to hold references to the ROOT.
	* References to Aggregate internals from the outer world are strictly forbidden.
	* Within the Aggregate Boundary [[Entities]] and [[ValueObject]] are allowed have references to each other, but they are only unique within the boundary.

![[aggregate-ddd-0.png]]

TODO: update images

```ad-quote
An Aggregate is a unit of consistency and concurrency!
```


![[aggregate-ddd-1.png]]


Aggregate is a distinct __Life-Cycle__ and has an __Identity__.

![[aggregate-ddd-2.png]]

An Aggregates are
* Part of the __domain__ and its __language__
	* Aggregates represent concepts from the ubiquitous language, and if not they should be introduced.
* Artificial __boundaries__ around __closely related models__.
	* These boundaries partition the domain model into closely related groups. And often we have to invent them.
* __Life-cycle__ with an __identity__
	* Aggregates are born and end at some point, going through a series of data changes on its way.
* __Units__ of __consistency__ and __concurrency__.
	* Aggregate ensure the consistency of changes for the domain concepts that are part of the domain.
	* One major drivers is managing concurrent access in large and complex software system.
* __Units__ of  __distribution.
	* The parts of Aggregates should be kept together closely since they are interacting very often.

![[aggregate-ddd-3.png]]


### How big should an Aggregate be?
As big as necessary and as small as possible.


```ad-quote
Domain-Driven Design often means deciding on that is inside and that is outside of boundaries and how these intract,using the knowledge of domain experts an heuristics to guide us.
```
