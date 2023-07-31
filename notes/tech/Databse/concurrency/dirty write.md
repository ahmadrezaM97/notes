# dirty write
Created: 2023-01-05 12:14
Tags: 
____

### Intro

What happens if two transactions concurrently try to update the same object ins  database, We don't know in which order the writes will happen, but we normally assume that the late write overwrite the earlier write.

However, what happens if the earlier write is part of a transaction that has not yet committed, so the later write overwrite an uncommitted value.

```ad-warning
title: increament?
However, read committed does not prevent the race condition between two counter increments. 
you will see that in 'LOST UPDATE'
```



![[dirty-write.png]]


#### Whats wrong with dirty write?

1. If transaction update multiple object, dirty write can lead to a bad outcome. 


### Who read committed prevent dirty write?

Most commonly, databases prevent dirty write by using row-level locks

when a transaction wants to modify a particular object(row or document)
It must first acquire a lock on the object It must the hold that lock until the transaction is committed or aborted,

Only one transaction can hold the lock for any given object

if another transaction wants to write to the same object, it must wait until the first transaction is committed or aborted.

This locking is done automatically by databases in read committed mode(or stronger isolation level).



### Postgres

`FOR UPDATE`

`FOR UPDATE` causes the rows retrieved by the `SELECT` statement to be locked as though for update. This prevents them from being locked, modified or deleted by other transactions until the current transaction ends. That is, other transactions that attempt `UPDATE`, `DELETE`, `SELECT FOR UPDATE`, `SELECT FOR NO KEY UPDATE`, `SELECT FOR SHARE` or `SELECT FOR KEY SHARE` of these rows will be blocked until the current transaction ends; conversely, `SELECT FOR UPDATE` will wait for a concurrent transaction that has run any of those commands on the same row, and will then lock and return the updated row (or no row, if the row was deleted). Within a `REPEATABLE READ` or `SERIALIZABLE` transaction

__The `FOR UPDATE` lock mode is also acquired by any `DELETE` on a row, and also by an `UPDATE` that modifies the values of certain columns__. Currently, the set of columns considered for the `UPDATE` case are those that have a unique index on them that can be used in a foreign key (so partial indexes and expressional indexes are not considered), but this may change in the future.

```ad-danger
title:dead-lock


```

```sql
# t1:
update person set age = 1 where id = 1;
```

```sql
# t2
 update person set age = 2 where id = 3;
update person set age = 2 where id = 3;
# locked

```

```sql
# t1 
update person set age = 19 where id = 3;

```

```
ERROR:  deadlock detected
DETAIL:  Process 437 waits for ShareLock on transaction 826; blocked by process 218.
Process 218 waits for ShareLock on transaction 825; blocked by process 437.
HINT:  See server log for query details.
CONTEXT:  while updating tuple (0,6) in relation "person"
```


##### References
1.

