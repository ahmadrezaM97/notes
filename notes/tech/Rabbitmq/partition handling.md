# partition handling
Created: 2023-01-05 20:31
Tags: 
____

### Intro

While a network partition is in place, the two (or more!) sides of the cluster can evolve independently.
With both side thinking the other has crashed. This scenario is known as split-brain.
Queues, binding, exchange can be created or deleted separatly.

### Quorum

When it comes to network partition quorum queues are much simpler

1. They use a separate and __much faster failure detector__ that can detect partitions rapidly and trigger fast leader elections meaning that availability is either not impacted or is quickly restored
2. The `cluster_partition_handling` configuration __does not apply to quorum queues__ 
	1. though the `pause_minority` mode can still affect a quorum queue as when minority side is paused, any quorum queue leaders  hosted on that node will become unavailable, 
		1. however due to the speed of the failure detector, in the event of a partition, a leader election should have selected a new leader well before this pause is triggered.

```ad-warning
title: data safety
data safety starts with applications doing the right thing.
using publisher confirms and consumer acks correctly
```
Quorum queues will elect a new leader on the majority side.
Quorum queue replicas on the minority side will no longer make progress ( accept new messages, deliver to consumer), all this work will be dome by the new leader.


### Mirrored Queue

classic mirrored queues which are split across the partition will end up with one leader on each side of partition, again with both side acting independently.

```ad-danger
title: SET 'partition handing strategy'

Unless a partition handling strategy, such as `pause_minority` is configured to be used, the split will continue event after network connectivity is restored.
```


##### Recovering From a Split-brain
ill restore state from the trusted partitions.

three ways to deal with network partitions automatically


`cluster_partition_handing` config:

1. `pause_minority`
2. `autoheal`
3. `pause_if_all_down`: 
	1. nodes : nodes which should be unavailable to pause
	2. recover: recover action
		1. ignore
		2. auto heal

which mode to pick?

1. `pause_if_all_down` with `ignore`
	1. use when network is reliability is the highest practically possible and node availability is of topmost importance.
	2. for example, all cluster nodes can be in the same rack or equivalent, connected with a switch, and that switch is also the route to outside world
2. `pause_minority`
	1. appropriate when clustering across racks or availability zones in a single region, and the probability of losing a majority of nodes at once is considered very low.
	2. this mode trades off some availability for the ability to automatically recover if the lost node come back
3.  `autoheal`
	1. appropriate when are more concerned with continuity of service than with data consistency across nodes



4. `pause-minority`
	1. rabbit will pause cluster nodes which determine themselves to be in a minority(i.e fewer or equal than the half the total number of nodes) 
	2. it choose partition tolerance(consistency) over availability from the [[CAP theorem]]
	3. the minority nodes will pause as soon as a partition  starts
	4. the minority nodes will start again when the partition ends
	5. This configuration prevent split-brain and is therefore able to automatically recover from network partition without inconsistencies
5. `pause-if-all-down`
	1. `rabbitMQ` will automatically pause cluster nodes which cannot reach any of the listed nodes
	2. all the listed nodes must be down to pause a cluster node
	3. this is close to the pause minority, however, it allows an admin to decide which nodes to prefer instead of relying on the context

If the cluster is made of two nodes in rack A and two nodes in rack B
and the link between racks is lost
-> pause minority: will pause all nodes.
-> in pause-if-all-down: if the admin listed the two nodes in rack A, only rack B will pause
	
1. `autoheal`
	1. `rabbitmq` will automatically decide on a winning partition 
	2. will restart all nodes that are not in the winning partition
	1. Unlike `pause_minority` it therefore takes effect when a partition ends, rather than when one starts. ( منظورم اینکه بع از اینکه پارتیشن ریستور شد معنی داره )
	2. __The winning partition is the one which has the most clients connected__


_____
##### References
1.

