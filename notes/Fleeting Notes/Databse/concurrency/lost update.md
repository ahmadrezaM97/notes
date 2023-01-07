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


solutions

1. Atomic write operations

for example the following instruction is concurrency-safe in most relational databases
```sql
 UPDATE counters SET value = value + 1 WHERE key = 'foo'
```

 ->document database such as MongoDB provide atomic operations for making lock modifications to a part of a `JSON` document
 -> Redis provides atomic operations for modifying data structions such a priority queues

not all writes can easily be expressed in terms of atomic operations, but in situations where atomic operations cab be used, they are usually the best choice.

Atomic operations are usually implemented by taking an exclusive lock 
_____
##### References
1.

