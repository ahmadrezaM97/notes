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




_____
##### References
1.

