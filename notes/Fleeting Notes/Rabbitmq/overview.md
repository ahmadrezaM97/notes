# overview
Created: 2023-01-03 20:53
Tags: 
____

__RabbitMQ is a distributed message queue system.__

-> __Distributed__
	It is usually run as a __cluster of nodes__ where queues are spread across the nodes and optionally replicated for __fault tolerance__ and __high availability__.

-> __Protocol__
	It natively implement [[AMQP]] 0.9.1 and offers other protocols such as AMQP 1.0, STOMP, MQTT and HTTP via plug-ins.

```ad-warning
title: what is its killer feature?

Building a fast, scalable, reliable distributed message xystem is an achievement in itself, but the __message routing functionality__ is what makes it truly stand out among they myriad of messaging technologies out there.
```


### Exchanges and Queues

The super simplified overview:

1. Publishers send messages to exchanges.
2. Exchange route messages to queues and other exchanges.
3. `RabbitMQ` sends acknowledgements to publishers on message receipt.
4. Consumers maintain persist TCP connections with `RabbitMQ` and declare which queue(s) they consume.
5. `RabbitMQ` __pushes__ messages to consumers.
6. Consumers send acknowledgements of success/failure
7. Messages are removed from queues once consumed successfuly

```ad-danger
title: Do not be fooled!
Exchange is _NOT_ a "thing".
----
A common misconception of exchange in rabbitmq are they are "things" that you send message to.

---
Infact that are routing rules.
____

You send message to a channel process, it uses the routing rules(exchange) to decide where to send the message on to.

```

![[rabbitmq-multipublisher-multiconsumer.png]]

#### Guarantees

__RabbitMQ offers "at most once delivery" and "at least once delivery" but not "exactly once delivery".

#### Order

Message are delivered in order of their arrival to the queue. This doesn't guarantee the completion of message processing matches that exact same order when you have competing consumers.
This problem can be resolved by using __Consistent Hashing Exchange__.

#### PUSH and CONSUMER PREFETCH

1. RabbitMQ pushes messages to consumers in a stream
2. There is a PULL API but it has terrible performance as each message requires a request/response round-trip
3. Push-based system can overwhelm consumers if messages arrive at the queue faster than consumers can process them -> to avoid this consumer can configure a prefetch limit (a.k.a QoS limit). This is basically is the number of acknowledged messages that a consumer can have at any one time.


```ad-warning
title: Why push and not pull?

1. First of all it is greate for low latency
2. Secondly, idealy when we hae competing consumer of a single queue we want to distribute load evenly between them. And push is better for that purpose.

If each consumer pulls messages then depending on how many they pull the distribution of work can get pretty uneven.
The more uneven the distribution of message the more latency and the further the loss of message ordering at processing time.
For that reason  RabbitMQ's PULL API only allow to pull on message at a time, but that seriously impacts performance.
```


#### Routing

```ad-note
title: what the heck are exchanges?
They are basically just routing rules for message
```

Type of exchanges

1. __Fanout__ 
	1. Routes to all queues and exchanges that have a binding to the exchanges. -> The standard pub sub model
2. __Direct__
	1. Route message based on __Routing Key__ that the message carries with it, set by the publisher.
	2. A routing key is a short string. Direct exchanges route messages to queues/exchanges that have __Binding Key__ that exactly matches the routing key.
3. __Topic__
	1. Routes messages based on a routing key, but allows wildcard matching.
4. __Header__
	1. RabbitMQ allows custom headers to be added to messages.
	2. 
```ad-note
title: routing key?
A routing key is a short string. 
```




_____

##### References
1.

