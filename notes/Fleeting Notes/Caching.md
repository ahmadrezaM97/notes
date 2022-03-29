# Caching
Created: 2022-03-31 02:06
Tags: #network_calls #recomputations #cache
____

##### Use cases Ideas
- Save network calls
	- Avoid load on databases
		- reduce db load
- Avoid Recomputations

> A ton of data in cache is useless

#### Cache policies
-   LRU - least recently used
-   LFU - least frequently used


#### Problems
- Poor revocation policy
	- use make useless extra call :|
- Trashing
	- ?
- Consistency 

#### where
- In memory in server
	-  couple with server 
	- Inconsistency
- closer to server is faster 
- closer to db in more reliable
- scaling cache independently

#### mechanism
- ### Write Through cache
	- write cache before writing to database
	- your are synchronously block
	- [[Cache aside]]
	- ![[cache-walk-through.png]]
- Write Back
	-  You first write to cache and then asynchronously write it to db
	- hit db and make sure cache is correct
	- performance problem
	- durability problem
-  hybrid
	- write to cache and after a while bulk update to databases


#### Type of cache
- Spatial Cache
	- predict you might need data spatially located
	- 
- Temporal (Least recently used cache)
	- [[CPU cache]] using this method (L)
- [[Distributed Cache]]
	- consistency problem



[[REDIS]]
_____
##### Refrences
1. [Gaurav Youtube](https://www.youtube.com/watch?v=U3RkDLtS7uY)
2. [Nasser Youtube](https://www.youtube.com/watch?v=ccemOqDrc2I)
