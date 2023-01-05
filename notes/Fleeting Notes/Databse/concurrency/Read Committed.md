# Read Committed
Created: 2023-01-05 12:15
Tags: 
____
### Intro

the most basic level of transaction isolation is read committed

It makes two guarantees:
	1. When reading from the database, you will only see data that has been committed. (no dirty read) [[dirty read]]
	2. When writing to the database, you will only OVERWRITE data that has been committed. (no dirty write) [[dirty write]]


Read committed is a very popular isolation level, it is the default setting in Oracle, PostgreSQL and many other database.



_____
##### References
1.

