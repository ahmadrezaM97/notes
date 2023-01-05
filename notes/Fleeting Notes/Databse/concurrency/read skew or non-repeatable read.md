# read skew or non-repeatable read
Created: 2023-01-05 22:34
Tags: 
____


![[read-skew.png]]

-> Say Alice has $1000 of savings at a bank, split across two accounts with $500 each
-> now a transaction transfers $100 from one of her accounts to the other
-> if she is unlucky enough to look at her list of account balances in the same moment as that transaction is being processed, she may see one accounts balances in the same moment as that transaction is being processed


this anomaly is called a `nonrepeatable read` or `read skew`

### What is wrong with read skew?

1. Backups
	1. Tacking backups requires making a copy of the entire database, which may take hours on a large database
	2. During the time that the backup process is running, write  will continue to be made to the database.
	3. you could end up with some parts of the backup container an older version of the data, and other parts containing a newer version
2. Analytic queries and integrity checks
	1. sometimes, you may want to tun a query that scan over large parts of data base, such queries are common in analytics or may be part of a periodic integrity check that every thing is in order( monitoring for data corruption )

### Solution

1. `Snapshot isolation` is the most common solution to this problem, the idea is that each transaction reads from a consistent snapshot of the database.
2. The transaction sees all the data that was committed in the database at the start of the transaction
3. Even if data is subsequently changed by another transaction, each transaction sees only the old data from that particular point of time.

implementing snapshot isolation


__Implementing snapshot isolation__

1. Like read committed isolation, implementations of snapshot isolation typically use `write locks` to prevent dirty writes. however, reads do not require any locks

```ad-quote
A Key principle of snapshot isolation is reader never block writer, and writers never block readers.
```

To implement snapshot isolation, database use a generalization of the mechanism for preventing dirty reads.
The database must potentially keep ser

This allows a database to handle long-running read queries on a consistent snapshot at the same time as processing writes normally, without any lock contention between the two.

_____
##### References
1.

