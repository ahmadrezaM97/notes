# Clustring
Created: 2023-01-04 04:11
Tags: 
____

### Intro
__We'll see that consistency and availability are at two ends of a spectrum and you'll need to choose which of those you'll optimize for.__

__There is a chain of responsibility between producers, broker and consumers__

 once a message delivered has been handed off to a broker, it is the broker's job not to lose that message
 
 when the broker acknowledges receipt of a message to the publisher, we don't expect that message to be lost




#### Publisher Confirms

Without publisher confirms it is possible to lose message

__A confirm is sent to a publisher once a message has been written to disk.__

`RabbitMQ` does not write message to disk on receipt but on a periodic basis, in the region of a few hundreds ms.

when a queue is mirrored then an ack is only sent once all mirrors have also written their copy of the message to dist. -> adds more latency


#### Clustering with Quorum Queues

__`Quoram` queues are a replicated queue based on [[Raft]] consensus algorithm__

A message is guaranteed not to be lost as long as majority of replicas are not permanently lost.

On a majority are lost, no guarantees are made.

A quorum queue has
1. __one leader that receive all reads and writes
2. one more followers that receive all message and meta data from the leader.

```ad-warning
These followers do not exist for scaling out the reading of queues but solely for redundancy.
```

![[quorum.png]]

__Leaders of multiple queues generally distributed across the cluster.__

![[quarum1.png]]

when a server goes down, any queues that had leader on that node need to elect a follower as the new leader.

__They can only do so if a majority of replica queues are still available.__

![[quoram2.png]]

### Resource Use

Quorum queues typically require more resources(ram and disk) than classic mirrored queues
To enable fast election of new leader and recovery.

All members in a quorum queue "cluster" keep all messages in the queue in memory and on disk

Quorum queues use a write-ahead-log (`WAL`) for all paratition.
`WAL` operations are stored both in memory and written to disk.
When the current `WAL` file reaches a predefined limit, it is flushed to a `WAL` `segment` file then compacted over time as consumers acknowledge deliveries.
Compaction is the process that reclaims disk space


As quorum queues persist all data to disks before doing anything it is recomended to use the fastest disk possible

### Data Safety

__quorum queues are designed to provide data safety under network partition and failure scenarios__

1. A message that was successfully confirmed back to the publisher using [[publisher confirms]] feature should not be lost as long as at leas a majority of nodes hosting the quorum queue are not permanently made unavailable
2. Generally quorum queues favours data consistency over availability.


```ad-danger
No Quarantees are provided for messages that have not been confirmed using the publisher confirm mechanism.

Such messages could be lost "mid-way" in an perating system buffer or otherwise fail to reach the queue leader
```



### When not to use Quorum Queues

in some cases quorum queues should not be used

1. temporary nature of queue
	1. transient or exclusive queues, high queue churn( declaration and deletetion rate)
	2. lowest possible latency
		1. the underlying consesus algorithm has an inherently higher latency due to its data safety feature
	3. when data safety is not a priority
		1. applications do not use manual acknowledgments and publisher confirms are not used
	4. very long queue backlogs ( strams are likely to be abtter fir)
	



https://jack-vanlightly.com/blog/2018/8/31/rabbitmq-vs-kafka-part-5-fault-tolerance-and-high-availability-with-rabbitmq


https://tanzu.vmware.com/content/webinars/jun-11-ha-and-data-safety-in-messaging-quorum-queues-in-rabbitmq?utm_campaign=Global_BT_Q221_RabbitMQ-Data-Safety-in-Messaging&utm_source=rabbitmq&utm_medium=website

Quorum queues use a write-ahead-log (WAL) for all operations. WAL operations are stored both in memory and written to disk. When the current WAL file reaches a predefined limit, it is flushed to a WAL segment file on disk and the system will begin to release the memory used by that batch of log entries. The segment files are then compacted over time as consumers [acknowledge deliveries](https://www.rabbitmq.com/confirms.html). Compaction is the process that reclaims disk space.


_____
##### References
1.

