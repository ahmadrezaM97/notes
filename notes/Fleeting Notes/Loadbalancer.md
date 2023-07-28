# Loadbalancer
Created: 2022-05-16 20:15
Tags: 
____

###### Introduction

__load balancing enables our service to scale well and stay highly available when the traffic load increase.__


![[load-blancer-1.jpeg]]


__Additional benefits include:

__SSL termination__
	Decrypt incoming request and encrypt server responses 
	So backend servers do not have to perfirms these potentially expensive operations.

__Session persistence__
	Issue cookies and route a specific client's requests to same instance if the web apps do 
	not keep track of sessions


__There are primarily three modes of load balancing
* DNS Load Balancing
* Hardware-based Load Balancing
* Software-based Load Balancing


###### Hardware-based Load balancing

Hardware load balancers are highly performant physical hardware.
-> Need maintenance and regular update
-> When it comes to performance, hardware load balancers stand out

###### Software-based load balancer
-> More cost-effective and offer more flexibility 
-> You can find loadbalancer as service(LBaas)
-> They are pretty advanced compared to DNS loadbalancer,
	-> They consider many parameters such 
		-> cookies | HTTP headers, CUP and memory utilization
		to route traffic across the servers.

```ad-tip

They also continually perform health checks on the servers to keep an updated list of in-service machinces.

To intelligently rout all the user requests to the active nodes in the cluster, a load balancer should be well aware of their running status.

Ideally, a load balancer maintains a list of machines that are up and running in the cluster in real-time.


```

![[load-blancer-2.jpeg]]


*HA PROXY



[[DNS load balancing]]












Load balancers distribute incoming client requests to computing resources such as application servers and databases.  In each case, the load balancer returns the response from the computing resource to the appropriate client.  Load balancers are effective at:

* Preventing requests from going to unhealthy servers
* Preventing overloading resources
* Helping to eliminate a single point of failure

Load balancers can be implemented with hardware (expensive) or with software such as HAProxy.

Additional benefits include:

* **SSL termination** - Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations
    * Removes the need to install [X.509 certificates](https://en.wikipedia.org/wiki/X.509) on each server
* **Session persistence** - Issue cookies and route a specific client's requests to same instance if the web apps do not keep track of sessions

To protect against failures, it's common to set up multiple load balancers, either in [active-passive](#active-passive) or [active-active](#active-active) mode.

Load balancers can route traffic based on various metrics, including:

* Random
* Least loaded
* Session/cookies
* [Round robin or weighted round robin](https://www.g33kinfo.com/info/round-robin-vs-weighted-round-robin-lb)
* [Layer 4](#layer-4-load-balancing)
* [Layer 7](#layer-7-load-balancing)

###### Layer 4 load balancing

Layer 4 load balancers look at info at the [transport layer](#communication) to decide how to distribute requests.  Generally, this involves the source, destination IP addresses, and ports in the header, but not the contents of the packet.  Layer 4 load balancers forward network packets to and from the upstream server, performing [Network Address Translation (NAT)](https://www.nginx.com/resources/glossary/layer-4-load-balancing/)

##### Methods

```ad-tip
title: round robin and weighted round robin

Thus approach is pretty useful when the services is deploted across multiple data centers having diffrent compute capacities.
More traffic can be directed to the larger data centers containt more moachines.

```


```ad-tip
title:Least connections
__The traffic is routed to the machine with least open connection of all the machines in the cluster

* First approach: It is assumed that all the requests will consume an equal amout of server resources
* Second approach: There is a possibility that the machine with the least open connections might already be processing requests demanding most of it's CPU power.


The least connection approach comes in handy when the server has long opened connections like persistent connections on a gaming applicaiton

```

```ad-tip
title: Random
```


```ad-tip:
title: HASH

* Hashing the source IP ensures that a client's request with a certain IP will always be routed to the same server

* Hashing the client ip also enables the client to re-establish the connection with the same server that was processing its request in case the connection drops.


* Hashing a URL ensures that the request wuth the URL always hits a certain cache that already has data on it.
  The is to ensure that there is no cache miss.
  
```

	

##### Loadbalancer and Horizontal scaling
load balanceres can also help with horizontal scaling

-> scaling horizontally introduces complexity
	-> Servers should be stateless: they should not containt any user-related data like sessions
	-> Sessions can be stored in a centralized data store such as database or a persistent cache
-> Downstream servers  such as caches and databases need to handle more  simultanous connections as upstream server scale our


###### Layer 7 load balancing

Layer 7 load balancers look at the [application layer](#communication) to decide how to distribute requests.  This can involve contents of the header, message, and cookies.  Layer 7 load balancers terminate network traffic, reads the message, makes a load-balancing decision, then opens a connection to the selected server.  For example, a layer 7 load balancer can direct video traffic to servers that host videos while directing more sensitive user billing traffic to security-hardened servers.

At the cost of flexibility, layer 4 load balancing requires less time and computing resources than Layer 7, although the performance impact can be minimal on modern commodity hardware.



#####  Disadvantage(s)

```ad-danger
title: Disadvantages

1-The load balancer can become a performance bottlenech if it does not have enough resources or if it not configured properly

2-A single load balancer is A SINGLE POINT OF FAILURE, configuring multiple load balancers further increases complexity

```


_____



_______________________________
##### References
1.

