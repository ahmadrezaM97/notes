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





_____

##### References
1.

