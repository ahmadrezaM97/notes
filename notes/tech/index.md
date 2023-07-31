# index
Created: 2023-01-02 17:12
Tags: 
____

### Primary index vs Secondary index
https://stackoverflow.com/questions/51083881/what-is-the-definition-of-secondary-index-in-postgresql

https://towardsdatascience.com/primary-key-and-clustered-index-bf85f7f87b60
-> A Primary index as an index on an ordered file where the search key is the same as the sort key
-> A secondary index provides a secondary means of accessing a data file for  which some primary access already exists. The data file records could be ordered, unordered, or hashed.


-> A primary index determines the location of the records of the data file

-> The secondary index is distinguished from the primary index in that a secondary index does not determine the placement of records in the data file. Rather, the secondary index tells us the current locations of records.




#### postgres

Postgresql utilizes a heap structure for the physical storage for records.
Heap are not sorted. Therefore, even the primary keys are implemented using secondary indices. and as such all indices in Postgresql are secondary.


All index in PostgresQL are secondary indexes 
meaning that each index is stored reparatly from the table's main data area



_____
##### References
1.

