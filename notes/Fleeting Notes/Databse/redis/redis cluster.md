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

-> __Each node in the cluster maintains a local copy of the "routing tables?__

-> __Continuously updates its routing table through gossip protocol__.
-

I



### introduction

Redis cluster is an active-passive cluster implementation that consist of master and slave nodes.

The cluster uses hash partitioning to split the keyspace into 16,384 key slots, with each master responsible for a subset of those slots

__Ports Communication__
Each node in a cluster requires two `TCP` ports.

1. One port is used for client connections and communications
	1. The is the port you would configure into client application or command-line tools
2. The second required port is reserved for node-to-node communaction
	1. That occurs in a binary protocol and allow the nodes to discuss configuration and node availability


### Failover

When a master fails or is found to be unreachable by the majority of the cluster as determined by the nodes communication via the gossip port
The remaining masters hold a vote and elect on of the failing masters' salves to take its place.


#### Rejoining The cluster

When the failing master eventually rejoins the cluster, it will join as a slave and begin to replicate another master

#### Sharding

redis sharded data automatically into the servers.

Redis has a consept of the `hash slot` in order to split data.
All the data are divided into slots

There are 16384 slots. These slots are divided by the number of servers

If there are 3 servers A,B and C then

* Server 1 contains hash slots from 0 to 5500.
* Server 2 contains hash slots from 5501 to 11000.
* Server 3 contains hash slots from 11001 to 16383




Redis scales horizontally with a deployment topology called Redis cluster.

With redis cluster, you get the ability to:
1. Automatically split your data set among multiple nodes
2. Continue operation when a subset of the nodes are experiencing failures or are unable to communicate with the rest of cluster

redis cluster is a data sharding solution with automatic management, failover.


_____
##### References
1.

