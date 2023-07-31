##### DNS load balancing

![[load-blancer-3.jpeg]]
* DNS load balancing enables the [[Authoritative server]] to return different IP addresses of a particular domain to the clients.
	* Every time it receives a query for an IP, it returns a list of IP addresses of a domain to the client.
	* Why every request, the authoritative server changes the order of the IP addresses in [[round-robin]] fashion.
	* as the client receives the list, it send our request to the first ip address on the list to fetch the data from the website.

``` ad-warning
title: why list?
the reason for returning a list of IP addresses to the client is to enable it to use other IP addresses in the list in case the first does'nt return a response with stipulated time.

``` 
``` ad-warning
title: It may be another loadbalancer
When the client hits an IP, it may not necessarily hit a application server. Instead, it may hi another loadbalancer implemented at the data center level
``` 

``` ad-danger
title: Limitation of DNS loadbalancing

1. Clients have own DNS caching so it's possible routed to a machine that is out of service.

2. IT's not That SMART :), (for instanse it doesn't know anything about current load fo machines)

```
