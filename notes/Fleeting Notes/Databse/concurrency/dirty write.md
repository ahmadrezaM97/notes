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



_____
##### References
1.

