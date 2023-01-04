# Clustring
Created: 2023-01-04 04:11
Tags: 
____

### Intro
__We'll see that consistency and availability are at two ends of a spectrum and you'll need to choose which of those you'll optimize for.__

__There is a chain of responsibility between producers, broker and consumers__

 once a message delivered has been handed off to a broker, it is the broker's job not to lose that message
 
 when the broker acknowledges receipt of a message to the publisher, we don't expect that message to be lost


### Durablelity

RabbitMQ has two types of queues
1. Durable
	1. all queues are peristed to the Msesia database.
	2. Durable queues are  redeclared on node start-up and so survive a restart, system crash or server failure( as long as the data survicves)
2. non-durable
	1. non-durable queues and exchanges are deleted on start-up.

```ad-warning
title: Presisst the Message
Just because a queue is durable __doesn't__ mean its message survive a node restart.
Only messages set as persistent by their publisher will be recovered.

```

![[rabbit-durability-matrix.png]]

#### publisher confirms

Without publisher confirms it is possible to lose message

A confirm is sent to a publisher once a message has been written to disk.

RabbitMQ does not write message to disk on receipt but on a periodic basis, in the region of a few hundreds ms.

when a queue is mirrored then an ack is only sent once all mirrors have also written their copy of the message to dist. -> adds more latency


#### Clustering with Quorum Queues

__Quoram queues are a replicated queue based on [[Raft]] consensus algorithm__

A message is guaranteed not to be lost as long as majority of replicas are not permanently lost.
On a majority are lost, no guarantees are made.


A quorum queue has
1. __on leader that recive

Quorum queues use a write-ahead-log (WAL) for all operations. WAL operations are stored both in memory and written to disk. When the current WAL file reaches a predefined limit, it is flushed to a WAL segment file on disk and the system will begin to release the memory used by that batch of log entries. The segment files are then compacted over time as consumers [acknowledge deliveries](https://www.rabbitmq.com/confirms.html). Compaction is the process that reclaims disk space.


_____
##### References
1.

