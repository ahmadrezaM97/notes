# lost update
Created: 2023-01-05 12:14
Tags: 
____

The [[Read Committed]] and [[repeatable read]] (snapshot isolation) levels have been primarily about the guarantees of what a read-only transaction can see in the presence of concurrent writes.

The lost update problem can occur if an application __reads some value from database, modifies it, and writes back the modified value(a read-modify-write cycle)__ 

if two transactions do this concurrently , one the modifications can be lost, because the second write does not include the first modification

1. Incrementing a counter or update an account balance
2. making a local change to a complex value
	1. e.g. adding an element to a list within a `JSON` document
3. two user editing a wiki  page at the same time


### Solutions

1. Atomic write operations
2. Automatically detect lost update ( postgres in repeatable-read isolation level do this)
3. Explicit lock( select for update)
4. compare-set 


#### Atomic write operations

for example the following instruction is concurrency-safe in most relational databases
```sql
 UPDATE counters SET value = value + 1 WHERE key = 'foo'
```

 ->document database such as MongoDB provide atomic operations for making lock modifications to a part of a `JSON` document
 -> Redis provides atomic operations for modifying data structure such a priority queues

not all writes can easily be expressed in terms of atomic operations, but in situations where atomic operations cab be used, they are usually the best choice.

Atomic operations are usually implemented by taking an exclusive lock  on the object when it is read so that no other transaction can reed it until the update has been applied.
This technique is sometimes known as `cursor stability` . Another option is to simply force all atomic operation to be executed on a single thread.

#### Explicit locking

The application can explicitly lock objects that are going to be updated
Then the application can perform a read-modify-write cycle, and if any other transaction tries to concurrently read the same object, it is forced to wait until the first read-modify-write cycle has completed.

```sql
BEGIN;
SELECT * FROM figures WHERE name = 'robot' AND game_id = 337 FORUPDATE;

-- check where move is valid, then update the position
-- of the piece that was returned by the prevous SELECT

UPDATE figure SET position = 'c4' WHERE id = 1237;

COMMIT;
```

```ad-note
title: FOR UPDATE
the `FOR UPDATE` clause indicates that the database should take a lock on all rows returned by this query.
```

This works, but to get it right, you need to carefully think about your application logic.

__It's easy to forget to add a necessary lock somewhere in the code, and thus introduce a race condition__

#### Automatically detecting lost updates

Atomic operations and locks are ways of preventing lost updates by forcing the read-modify-write cycles to happen sequentially.

An alternative is to allow them to execute in parallel and, it the transaction manager detects a lost update, abort the transaction and force it to retry its read-modify-write cycle.

__An advantage of this approach is that databases can perform this check efficiently in conjunction with snapshot isolation__

__PostgreSQL's repeatable read and SQL Server's snapshot isolation level automatically detect when a lost update has occurred and abort the offending transaction.__

MySQL and InnoDB's repeatable read doesn't prevent lost update.

some authors argue that a database must prevent lost updates in order to qualify as providing snapshot isolation, so `MySQL` does not provide snapshot isolation under this definition.

#### Compare-and-set

In databases that don't provide transactions, you sometimes find an atomic compare-and-set operation 
Th purpose of this operation is to avoid lost updates by allowing an update to happen only if the value has not changed since you last read it

```sql
-- This may or maynot be safe, depending on the database implementation

UPDATE wiki_pages SET content = 'new content' WHERE id = 1233 AND content = 'old content'
```




### Conflict resolution and replication

In replicated database, preventing lost updates takes on another dimension.
since they have copies of the data on multiple nodes, and the data can potentially be modified concurrently on different nodes

=> some additional step need to be taken to prevent lost update.

1. Lock and compare-and-set operations assume that there is a single up-to-date copy of the data
	1. However, database with multi-leader or leaderless replication usually allow several writes to happen concurrently and replicate them asynchronously
	2. so they can not guarantee that there is a single up-to-date copy of the data
	3. see [[linerizability]]


A common approach in such replicated databases is to allow concurrent writes to create several conflicting versions of a value (a.k.a `sibling` ) and to use application code or special data structure to resolve and merge these version after the fact.


_____
##### References
1.

