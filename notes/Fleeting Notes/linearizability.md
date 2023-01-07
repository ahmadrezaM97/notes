# linearizability
Created: 2023-01-07 21:44
Tags: 
____

### Introduction

multiple nodes concurrently accessing replicated data.
How do we define "consistency" here?

The strongest option: __linearizability__

1. informally: every operation takes effect __atomically__ sometime after it started and before it finished

2.  All operations behave as if executed on a __single copy__ of the data
		1. (even if there are in fact multiple replicas)

3. Consequence 
		1. every operation returns an "up-to-date" value, a.k.a "strong consistency"
		
4. Not just in distributed system, also in shared-memory concurrency 
	1. memory on multi-core CPUs in not ls not linearizable by default


```ad-warning
title: linearizablity != serializability
serializability: about transaction execution
linearizability: behave like there is only one single copy of data
```


#### Read-after-write consistency 

 

_____
##### References
1.

