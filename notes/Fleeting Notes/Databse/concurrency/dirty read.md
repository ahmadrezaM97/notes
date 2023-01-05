# dirty read
Created: 2023-01-05 12:14
Tags: 
____
Imagine a transaction has written some data to the database, but the transaction  has not yet committed or aborted.
Can another transaction see the uncommitted data? 
If yes that is called a dirty read


### What wrong with dirty read

1. If a transaction need to update several object, a dirty read means that another transaction may see some of the updates but not others. and that my cause other transaction to take incorrect decisions.
2. If a transaction abort, any write it has made need to be rolled back, if the database allows dirty reads, that means a transaction may see data that is later rolled back.

![[dirty-read.png]]

![[dirty-read.jpeg]]


#### How to prevent dirty reads?

locks? again?
	 one option would be to use the same lock, and to require any transaction that wants to read an object to briefly acquire the lock and then release it again immediately after reading.
		read couldn't happen while an object has a dirty, uncommitted value ( because during that time the lock would be held by the transaction that has made the write)
	  the approach of requiring read locks does no work well in practice, because one long-running write transaction can force many read only transaction to wait

Most databases prevent dirty by remembers both the old committed value and the new value set by the transaction that currently hold the write lock
	while the transaction is ongoing, any other transactions that read the object are simply given the old value.
	Only when the new value is committed do transaction switch over to reading the new value.


![[dirty-read-0.png]]

_____
##### References
1.

