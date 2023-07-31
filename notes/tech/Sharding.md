# Sharding
Created: 2022-05-27 04:13
Tags: 
____
#### Outline
```ad-note
title: Why partitioning

Partitioning is necessary when you have so much data that storing and processing it on a singe machine is not longer feasible.


The goal of partitioning is to spead the data and query load evenlty across multiple machnies.

Avoiding hot Stops.

This requires choosing a partitioning scheme that is appropriate to your data.
And rebalancing the partitions when nodes are addred to or removed from the cluster.
```


```ad-note
title: To Main Approaches to partitioning

A. __Key Range Partitioning__ :
	1.  Where keys are sorted, and partion owns all the key from some minimum up to some maximum.
	2. Soring has the advantage that efficient range queries are possible.
		BUT: There is a risk of hot spots if the application often accesses keys that are close together in the sortedorder.
	3.In this approach, partions are typically reblanaced dynamically by splitting the range into two subrange.

B. __Hash Partitioning__
	1 Where a hash function os appliced to each key, and a partition owns a range of hashes.
	2 This method destroys the ordering of each key, makeing range queries inefiicient, 
		BUT: my distribute load more evenly.
	3 In this approach, it is common to create a fixed number of paritions in advance, to assign several partitions to each node.
		AND: moveentire partitions from one node to another when nodes are added or removed.
	4 Dynamic partitioning can also be used.

C. __Hybird approches are also possible__
	. for instanse : a compound key using one part of the key to identify the partition and another part for the sort order.

```

``` ad-note
title: [[secondary_indexes]] also needs to be paririoned, methods

Document-partitioned Indexes(Local Indexes)
	. Where the Secondary indexes are stored in the same partition as the primary key and value.
	. Only a single partition needs to be updated on writes.
	. BUT In read of the secondary index reques a scatter/gather across all partitions.

Term-paritioned indexes(Global Indexes)
	. Where the secondary indexes are partitioned separately, using the indexed values.
	. And entry in the secondary index may include records from all partions of the primary key
	. When a document is writen, serveral partitions of the scondary index need to be updated.
	. A read can be served from single partions.
```

```ad-note
title: Techniques for Routing Queries 

By design, every partition operates mostly independently 
thats what allows a partitioned database to scale to multiple machines.

Operations that need to write to serveal partitions cab difficult to reason about:
__What happens if the write to on partition success but another fail__


```

#### Introduction

```ad-tip
title: Terminological confusion

What we call a `partition` here is called a `shard` in `MongoDB` and `Elasticseach`
`vnode` in `Cassandra`
```

```ad-question
title: What is a partition?

Each Partiotion is a small database of its own, although the database my support operations that touch multiple partitions at the same time.


For very large datasets, or very hight query throughput, that is not sufficient
__We need to break the data up into `partitions` also known as `sharding`__

```

```ad-question
title:Why we nead partitioning
The main reason is [[scalability]]
```
```ad-note
title:Each node can independently execute the queris for its own partition.


-> Query throughput can be scaled by adding more nodes.

-> Large,Complex queries can potentially be parallelized across manu nodes, although this gets significantly harder.

```

#### Partitioning vs Replication

__Partitioning is usually combined with replication so that copies of each partition are stored on multiple nodes__

__It means that, even though each record belongs to exactly one partition, it may still be stored on several different nodes for fault tolerance__

```ad-warning
The choice of partitioning scheme is mostly independent of the choice of replication scheme.

```
![[partitioning-with-replication.png]]

#### Partitioning of Key-Value Data
```ad-question
title: How do you decide which recoreds to store on which nodes?

Our goal with partitioning is to spread the data and the query load evenly across nodes.

```
```ad-note
title: `skewed`

If the partitioning is unfair.
	so that some partitions have more data or queries than others, we call it `skewed`
	
```
```ad-note
title: hot spot
A partition with disproportionately high load is called a hot spot.
```

```ad-tip
title: Why is not a good idea to spead data randomly?

when you are trying to read a particular item, you have no ways of knowing which node it is on.

SO: you have to query all nodes in parallel.

We can do better. Cann't we?

```

#### Partitioning by Key Range

```ad-question
title: How deos it work?

If you know the boundaries between the ranges, you can easily determine with parition contains a given key.

If you also know wich parition is assigned to which node, then you can make your request directly to the apporpriate node.
```
```ad-warning

The ranges of keys are not necessarily evenly spaced.
BECAUSE: your data may not be evenly distributed.

```

```ad-tip
title: Which databases use this approach?

MongoDB
HBase

```

```ad-note
title: A great IDEAD

Within each partition, we can keep keys in sorted order ([[SSTables and LSM-trees]])

Advantage:
	__Range scans are easy, and you can treat the key as a concatednated index in order to fetch several related records in on query__

Downside:
	__Certain access patterns can leat to hot spot__
```
```ad-example
title: Sensores Problem and another great idead

Consider an application that stores data from network of sensors, where the key is timestamp of the measurement.

Range scans are very useful in this case, because they let you easily fetch, say, all reading from a particular month.

However, the downside of this approach is that certain access patterns can lead to hot spots if the key is a timestamp, the the partitions correspond to ranges of time.

To avoid this problem in the sensor database, you could prefic each timestamp with the sensor name so that the partitioning is first by sensor name and the by time.

When you want to fetch all values of mulitiple sensors within a time range, you need to perform a separte range query for each sensor name.

```

####  Partitioning by Hash of Key

__Because of this risk of skew and hot spots, many distributed datastores use a hash function to determine the partition for a given key__

```ad-warning:
For partitioning purposes, the has function need not be cryptographically strong:


Cassandra and MongoDB use MD%

```

[[Consistent Hashing]]

```ad-note

Unfortunately however, by using the hash if the key for partitioning we lose a nice property of key-range partitioning:

__The abiltity to do efficiend range queues__


In `MongoDB` is you have enabled hash-based sharding mode, any range query has to be sent to all partitions.

`Cassandra` achieves a compromise between the two paritioning strategies:
	A table in Cassandra can be declared with a compound primary key consisting of several columes.
	Only the first part of theat key is hashed to determine the partition
	Other columns are used as a concatenated index for sorting the data i ncassandra;s SSTables
	
	A query therefore cannot search for a range of values within the first column of a compound key, but if it specifies a fiexed value for the first column it can perfirm an efficient range scan over the other columns of the key

```

```ad-attention

The concatented index approach enable an elegant data model for on to may realationships.
For example, on social media site, one user may post may updates.
If the primary key for updates is chosen to be `(user_id, update_timestamp)`.

Then you can efficiently retrieve all updates made by a particualr user within some time intervals, sorted by tiemtamp.

Diffrent users may be stored on diffrenet paritions, byt within each user, the updates are stored ordered by tiemstamp on single partition.


```
_____
##### References
1.

