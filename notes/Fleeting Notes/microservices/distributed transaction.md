# distributed transaction
Created: 2022-12-30 03:29
Tags: 
____

```ad-note 
title: What is a distributed transaction?

. When a microservice acrchiecure decomposes a monolithic system into seld-encapsulated services, it can break `tranastion`


. This means a `local tranaction` in the monolithic system is now distributed into multiple services that will be called i na sequence.

```

### Example 

Here is a customer order example in a monolithic system using a local transaction:

![[distributed-transaction-0.png]]

here we are working with two table in a single transaction and if any step fails, the transaction can `roll back`. This is known as [[Atomicity]] in [[ACID]].


BUT when we decompose this system, we created both `CustomerMicroservice` and `OrderMicroservice`, which have separate databases

![[distributed-transaction-1.png]]


```ad-danger
title: What is the PROBLEM?

in a monolithic system, we have a database system to ensure ACIDity.
We now need to clarify the following key problems

1. How do we keep the transaction __atomic__?
	all steps complete or no steps complete
	
2. Do we __isolate__ user actions for __concurrent request__?
	if an object is written by a tranaction and at the save time, it is read by another request, should the object return old or updated data??
	

```

__Possible solution__
1. [[2PC]] ( two-phase commit)
2. [[Sage]]

### Two-phase commit pattern

There is two phases here 
1. A prepare phase
2. A commit phase

In the prepare phase, all `microservices` will be asked to prepare for some data change that could be done atomically.

Once all `microservices` are prepared, the commit phase will ask all `microservices` to make the actual change

There needs to be a `golabl coordinator` to maintain the lifecycle of the transaction, and the coordinator will need to call the microservices in the prepare and commit phases.

![[2pc-0.png]]

![[2pc-1.png]]

#### Benefits of using 2PC
I works :D
#### Disadvantages of useing 2PC

__it is not really recommended for many microservice-based systems__ because 
1. 2PC is synchronous (blocking)
		1. The protocol will need to lock the object that will be changed before the transaction completes.
		2. This is not good. In a db, transactions tend to be fast-normally within 50 ms
			1. The lock could become a system performance bottleneck
		3. __There is a chance of [[dead-lock]].__



#### SAGA

```ad-note
title: what is saga in a nutshell?
saga is a message-driven sequence of local transaction to maintain data consistancy.
```

![[data-consitancy-in-distributed-transaction-0.png]]

The traditional approach to maintaining data consistency across multiple services, databases or message broker is to use distributed transactions.

The the facto standard distributed transaction management uses [[2PC]] to ensure that all participants in a transaction either commit or rollback.

__Problems with [[2PC]]

1. Many model technologies, including NOSQL databases such as MongoDB and Cassandra, don not support them. Also, distributed transaction aren't supported by modern message broker such as `RabbitMQ` and `Apche Kafka`.
2. Another problem with distributed transactions is that they are a form of `synchronous` `IPC`, which reduces availability.

__What is Saga pattern

-> Sage are mechanism to maintain data consistency in a microservice architecture without having to use distributed transactions.

-> Sage is a sequence of local transactions, Each local transaction update data within a single service using familiar [[ACID]] transaction frameworks and libraries.

__Example
![[saga-example-0.png]]

This saga consists of the following local transactions:
1. Order Service -> Create an order in an `APPROVAL_PENDING` state.
2. Consumer Service -> Verify that the consumer can place an order
3. Kitchen Service -> Validate order details and create a `Ticket` in the `CREATE_PENDING`
4. Accounting Service -> Anuthorize consumer's credit card.
5. Kitchen Service -> Change the state of the `Ticket` to `AWAITING_ACCEPTANCE`
6. Order Service -> Change the state of the `Order` to `APPROVED`


__First Challenge: ROLLBACK

__Sage use compensating transaction to roll back changes__

-> A great feature of traditional ACID transactions is that the business logic can easily roll back a transaction it it detects the violation of a business rule.

![[saga-compensating-transaction-0.png]]

The sage executes the compensation transaction ins reverse order.



__Coordinating sagas__

-> __Choreography__
	Distributed the decision making and sequencing among the saga participants.
	The primarily communicate the exchanging event
![[saga-choreography.png]]

__Advantage__


-> __Orchestration__
	Centeralize a saga's coordination logic in a saga orchestrator class.
	A sage orchestrator sends command messages to sage participants telling them witch 
	operation to perform.

![[saga-orchestration.png]]

-> __Benefits__
1. __Simpler Dependencies
	1. The saga orchestrator invokes the saga participants, but the participants don't invoke the orchestrator. => the orchestatror depends on the participants but not vice versa. and there are no cycle dependency
2. __Less Coupling
	1. each service just has to implement  API that is invoked by the orchestator, so it doesn't know about other events.
3. __Improve sepration of concerns and simplifies the business logic_
	1. The saga coordination logic is localized in the saga orchestrator.
		1. => the domain objects are simpler and hve no knowledge about the sagas participants
		2. 
https://developers.redhat.com/articles/2021/09/21/distributed-transaction-patterns-microservices-compared#how_to_choose_a_distributed_transactions_strategyp


_____
##### References
1.

