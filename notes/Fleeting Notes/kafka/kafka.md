# kafka
Created: 2023-03-19 17:24
Tags: 
____
### introduction

1. It's about real-time experience 
2. Apache Kafka is an open source, distributed streaming platform it's allow for the development  of real-time event-driven applications.
3. produce and consumer stream and data records
4. characteristic
	1. it's distributed
	2. fast
	3. high accuracy 
	4. in order
	5. fault tolerant
5. use cases
	1. decouple
	2. messaging
	3. location tracing
	4. data gathering
6. 4-core API
	1. __producer__ API
		1. make the stream of data
		2. produce data to __TOPIC__
		3. topic is ordered list of event
			1. it's can save for an hour or minutres or forever
	2. __consumer__ API
		1. subscribe to one or more topic
		2. in real-time or old data
	3. __streams__ API
		1. is very powerful
		2. consume from a topic or topics
		3. analyze, aggregate and otherwise transform data and produce to new topic or same topic
	4. __connector__ API
		1. re



### why KAFKA is fast

#### sequential access pattern
* One of the reasons Kafka is fast( has a high throughput) is Kafka use sequential I/O
	* append only log as primary data structure 
	* access patterns
		1. random 
		2. sequential 


####  zero copy principle

[[zero copy]]

![[kafka-zero-copy-0.png]]

![[kafka-zero-copy-1.png]]


#### Kafka Broker

producer 
broker
consumer

* __Topic__
	* 
	