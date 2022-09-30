# Untitled 7
Created: 2022-09-06 20:13
Tags: 
____


### Pull vs Push

#### RabbitMQ uses a push model

- Prevents overwhelming consumers via the consumer configured prefetch limit.
- This is great for low latency messaging


#### Kafka uses a pull model


- consumers request batches for messages from a given offset

```ad-warning
title: long-polling

__To avoid tight loops when no messages exist beyond the current offset Kafka allows for long-polling__
```

- A pull model makes sense for Kafka due to partitions:
	- Kafka guarantees message order in a partition with no computing consumers
	-  => we can leverage the batching of messages for a more efficient message delivery that gives us higher throughout.
* This DOESN'T make so much sense for RabbitMQ:
	* as ideally we want to try to distribute messages one at a time as fast as possible to ensure that work is parallelized evenly 
		* And messages are processed close to the order in which they arrived in the queue.


#### Publish Subscribe


