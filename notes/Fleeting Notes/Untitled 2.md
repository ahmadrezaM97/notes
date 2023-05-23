

https://refactoring.guru/design-patterns/chain-of-responsibility/go/example



### Aggreagate

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
