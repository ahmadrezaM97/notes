# write skew
Created: 2023-01-03 03:25
Tags: 
____
[[dirty write]] and [[lost updates]] are two kinds of race conditions, but we also have THE WRITE SKEW.

Intuitive example

![[write-skew-example.png]]

Imagine the Alice and Bob are the two on-call doctors for a particular shift. both are feeling unwell, so they both decided to request leave.
In each transaction, your application first checks that two or more doctors are currently on call.
if yes, it assumes it's safe for one doctor to go off call.

Since that base use  snapshot isolation, both checks return 2, so both transaction commit. WTF


#### Characterizing write skew

It is neither a dirty write nor a lost update, because the two transaction are updating two different objects.

```ad-note
title: new prespective
You can think of write skew as a `generalization` of the lost update problem.


Write skew can occur if two transactions read the same objects and then update some of those object.

`In the special cas`e where two different transaction update the same object, you get a `dirty write` or `lost update` anomaly
```


-> Atomic single-object operations don't help, as multiple object are involved.
-> The automatic detection of lost update that you find in some implementations of snapshot isolation unfortunately doesn't help either.

__If you can not use a serializable isolation level, the second-best option in this cases is probably to explicitly lock the tow that the transaction depends on__

```sql
BEGIN TRANSACTION;

SELECT * FROM doctors 
	WHERE on_call = true
	AND shift_id = 1234 FOR UPDATE;

UPDATE doctors SET on_call = false WHERE name = 'Alice' AND shift_id = 1234;

COMMIT;
```


```ad-tip
title: select for update
FOR UPDATE telss the database to locks all rows returned by the query.
```

[[django select_for_update]]

-> In postgres document
`FOR UPDATE` cause the toes retrieved by the `SELECT` statement to be locked as thought for update.
This prevents them from being locked, modified or deleted by other transaction until the current transaction end.

[[postgres locks]]



#incomplete

#### More example of write skew

1. meeting room booking system.
2. multiplayer game
3. claiming a username
4. preventing double-spending
5. 




_____
##### References
1.

