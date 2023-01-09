# redis cluster
Created: 2023-01-07 22:28
Tags: 
____


Very soon the data volume, read/write qps will exceed the capacity of a single redis instance

in general there are three ways to shard the keys to multiple instances

#### Client-side sharding


example;
say we have an ad-ranking engine
-> essentially is a recommendation service that recalls relevant ads  from ads DB and ranks ads for each ad_request.
-> the rank engine need to retrieve real-time bid of each ad in order to perform ad auction.
-> Due to the highly latency-sensitive nature of the business, all readl-time bid of ads are computed and pre-loaded to a redis cluster

![[redis-shard-client-side.png]]

in client-side sharding, the redis client contains the sharding & routing logic.
In other words, it is aver thick client
advantage
	architecture doesn't rely on any middleware, there are only two parties	1. Redis client
	2. Redis nodes



#### server side sharding with centeralized proxy (aka middleware)

a middleware is used as proxy.
requests from ad-ranking engine will hit the proxy.
The proxy contains the sharding & routing logic to determine which redis instance to visit to retrieve data.

![[redis-sharding-with-proxy.png]]


#### Decentralized server-side sharding

Decentralized sharding is what actually used in official Redis clusters.

1. __Each node in the cluster maintains a local copy of the "routing tables?__

2. __Continuously updates its routing table through gossip protocol__.
3. there is no logger a centralized proxy, rather, each Redis node contains the full set of information needed to act as proxy.

4. A request can hit any Redis node.
5. Every nodes knows all the other nodes in the cluster in terms of
	1. network addresses
	2. what keys are stored
	3. etc
6. the node handling request will first check if itself or another node has the requested data, and then re-direct the request to the appropriate node if the data is stored elsewhere.

![[redis-decentralized-server-side-sharding.png]]


### Sharding algorithms

#### Consistent hashing

consistent hashing is a classical sharding algorithm.
[[Consistent Hashing]]


#### slot sharding

Redis cluster uses hash slot.

all the keys in the key space are hased into an integer range 0~ 16383
following formula `slot = CRC16(key) & 16383`

Each node is responsible for storing a set of slots, and the associated k-v pairs of the slots.

![[redis-hash-slot.png]]

in essence, the hash slot is another layer of abstraction.
It decouples data records (kv pairs) and nodes.

each node only need to know what slots should be stored on it.
slots are encoded as bit array


### introduction

Redis cluster is an active-passive cluster implementation that consist of master and slave nodes.

The cluster uses hash partitioning to split the keyspace into 16,384 key slots, with each master responsible for a subset of those slots

__Ports Communication__
Each node in a cluster requires two `TCP` ports.

1. One port is used for client connections and communications
	1. The is the port you would configure into client application or command-line tools
2. The second required port is reserved for node-to-node commucication
	1. That occurs in a binary protocol and allow the nodes to discuss configuration and node availability


### Failover

When a master fails or is found to be unreachable by the majority of the cluster as determined by the nodes communication via the gossip port
The remaining masters hold a vote and elect on of the failing masters' salves to take its place.




_____
##### References
1.

