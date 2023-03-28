# DDD
Created: 2023-03-28 13:36
Tags: 
____

Software development that centers the development on programming a domain model that has rich understanding on the rules of the business.

* Used to decompose complex business logic
* Start with ubiquitous language, a common vocabulary that is shared between all stakeholders.
* identifies relevant modules in the monolithic application and apply the common vocabulary to them.
* Domain services handle business logic isolated
* Building blocks
	* __Domain model__
		* all the rules and logic inside the business 
		* Domain is the problem in the business
		* "Model" is the solution for the problem
		* Organized and structured knowledge of the problem
		* The domain model is an abstract model of the business domain
	* __Entities__
		* with data and behavior of the domain model
		* contain all business logic pertaining ti an entity
		* Have identity, which will remain throughout its lifetime
		* example
			* Order
				* attributes
					* First name
					* last name
					* address
				* methods
					* Add order item
					* set address
	* Value object 
		* Have no identity
		* Don't live on their own
		* Attributes of an entity
		* Hold its own business logic
		* The customer address in an order in an example
	* aggregate root
	* aggregate
	* different services

* Bounded Context
	* context 
		* the setting in which a word or statement appears that determines its meaning
	* bounded context
		* the conditions under which a particular model is defined and applicable
* Models need  to be clear not big
	* Useful models need crisp definitions
	* Definitions require clear context
	* Useful models need simple assertions
	* Assertions require boundaries


_____
##### References
1.

