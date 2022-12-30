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
2. A commit pahse

1. in the prepare phase, all `microservices` will be asked to prepare for some data change that could be done atomically.
2. Once all `microservices` are prepared, the commit phase will ask all `microservices` to make the actual change



_____
##### References
1.

