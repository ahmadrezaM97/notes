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




_____
##### References
1.

