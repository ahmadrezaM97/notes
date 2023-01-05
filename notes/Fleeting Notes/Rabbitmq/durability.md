# durability
Created: 2023-01-05 21:05
Tags: 
____
#### Quorum

Regular queues can be non-durable, __Quorum__ queues are __always__ __durable__ per their assumed use cases.

messages published to the source quorum queue are persisted on disk, regardless of the message `delivery mode`(transient or persistent)
However messages that are dead lettered by the source quorum queu will keep the original message delivery mode.


laze mode 
1. since 3.10
	1. Quorum queues store their message content on disk( per Raft requirements) and only keep a small metadata record of each message in memory.
	2. lazy_mode_configuration doesn't apply
2. before 3.10
	1. quorum queues store their content on disk( per Raft requirements) as well as in memory( up to the in memory limit confiqured)




### Mirrored Queue
`RabbitMQ` has two types of queues 
1. Durable
	1. all queues are persisted to the Msesia database.
	2. Durable queues are  redeclared on node start-up and so survive a restart, system crash or server failure( as long as the data survives)
2. non-durable
	1. non-durable queues and exchanges are deleted on start-up.

```ad-warning
title: Presisst the Message
Just because a queue is durable __doesn't__ mean its message survive a node restart.
Only messages set as persistent by their publisher will be recovered.
```

![[rabbit-durability-matrix.png]]

Durable queues will be recovered on node boot, including messages in them published as persistent.
Messages published as transient will be `discarded` during recovery, even if they were stored in durable queues.





_____
##### References
1.

