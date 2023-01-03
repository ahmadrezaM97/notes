
# Two-Phase locking(2PL)
Created: 2023-01-03 03:07
Tags: 
____

The are three approach to implement [[Serializablitity]]

1. Literally executing transaction in a serial order ( [[Actual Serial Execution]])
2. Two-phase locking
3. Optimistic concurrency control techniques (serializable snapshot isolation)


There are two types of locks available `Shared S(a)` and `Exclusive X(a)`.

with 2PL we achieve serializability 

A transaction is said to follow the two-phase locking protocol if locking and unlocking can be done in two phases.

```ad-tip
title: Growing Phase
New locks on data items may be acquired but none can be released
```

```ad-tip
title: Shirinking phase
Existing locks may be released but no new lock can be acquired.
```


Several transactions are allowed to concurrently read the same object as long as nobody is writing to it, but as soon as anyone wants to write (modify or delete) an object exclusive access is required.

* If transaction `A` has read an object and transaction `B` wants to write to that object, `B` must wait until `A` commit or aborts before it can continue.
* If transaction `A` has written an object and transaction `B` wants to read that object, `B` must wait until `A` commit or aborts before it can continue.

In 2PL, writes don't just block other writers, they also blocks reader and vice versa.

```ad-danger
title: snapshot isolation vs 2PL
Snapshot isolation has the mantra readers never block writers, and writers never blcok readers, wich capture this key diffrenece between snapshot isolation and two-phase locking.
```


two-phase locking provide SERIALIZABILITY, it protects against all the race conditions, including [[lost updates]] and [[write skew]].






_____
##### References
1.

