# dirty read
Created: 2023-01-05 12:14
Tags: 
____
Imagine a transaction has written some data to the database, but the transaction  has not yet committed or aborted.
Can another transaction see the uncommitted data? 
If yes that is called a dirty read

![[dirty-read-0.png]]


### What wrong with dirty read

1. If a transaction need to update several object, a dirty read means that another transaction may see some of the updates but not others. and that my cause other transaction to take incorrect decisions.
2. If a transaction abort, any write it has made need to be rolled back, if the database allows dirty reads, that means a transaction may see data that is later rolled back.

_____
##### References
1.

