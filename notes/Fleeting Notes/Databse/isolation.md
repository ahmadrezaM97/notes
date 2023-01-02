# isolation
Created: 2023-01-02 17:40
Tags: 
____

1. Read committed
2. Read Uncommitted
3. Repeatable Reads
4. Serializable


__Read Committed Isolation Level__

```ad-note
title: no dirty read
This means the database will not read any uncomitted values.
```

![[dirty-read.png]]

```ad-note
title: not dirty write
This means that the database will accept any transaction on a particular row that is already having a transaction running on it.
The other transaction has to wait till the point the pervios transaction on the rows is committed and only after that, any other transaction will be able to perform a write operation for the specific rows.
```

```ad-note
title: Lost update
The problem occurs when there are two transaction onvolved and both are unaware of each other the trnasaction created by the earlier update.
```

```ad-note
title:Nonrepeatable Reads
This problem occurs when two `SELECT` statements of the same data results in diffrent values because another transaction has updated data between two `SELET` statement.
```

_____
##### References
1.

