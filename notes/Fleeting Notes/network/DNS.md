# DNS
Created: 2022-05-16 20:13
Tags: 
____


## Domain name system

### Why DNS

1. People can not remember IPs
2. A domain is a text points to an IP or a collection of IPs
3. Additional layer of abstraction is good
4. IP can change while the domain remain
5. We can serve the closest IP to a client requesting the same domain
6. Load balancing ( client side load balancing)

### DNS

1. A new addressing system means we need a mapping, Meet DNS
2. If you have an IP and  you need the MAC, we user [[ARP]]
3. If you have the name you need the IP, we use DNS
4. Build on Top of [[UDP]]
5. Port 53
6. Many Records(MX, TXT, A , CNAME)

#### How DNS WORKS

* DNS resolver -> frontend and cache
* ROOT Server -> Hosts IPs of TLDs(top level domain servers)
* Top Level domain server -> host IPS of ANS(authoritative name server)
* Authoritative Name Server -> Hosts the IP of the target server.
 
![[dns-0.png]]

1. DNS recursor
	1. The recursor can be thought of as librarian who is asked to go find a particular book somewhere in a library.
	2. The DNS recursor is a server designed to receive queries from client machines through application such as web browsers.
	3. then it responsible for making additional requests in order to satisfy the client's DNS query.
2. Root name server
	1. the root server is the first step in translating(resolving) human readable host names into IP addresses.
	2. It can be thought of an index in a library that points to different racks of books.
3. TLD name server
	1. The top level domain server can be thought of as a specific rack of books in a library.
4. Authoritative name server
	1. The final nameserver can be thought of as a dictionary on a tack of books.

![[dns-3.png]]




### DNS packet


![[dns-1.png]]


### DNS queries


### DNS Records


* **NS record (name server)** - Specifies the DNS servers for your domain/subdomain.
* **MX record (mail exchange)** - Specifies the mail servers for accepting messages.
* **A record (address)** - Points a name to an IP address.
* **CNAME (canonical)** - Points a name to another name or `CNAME` (example.com to www.example.com) or to an `A` record.


### nslookup


### DNS cache

* The purpose of caching is to temporarily stored data in a location that results in improvements in performance and reliability for data request.
* DNS data can be cached in a variety of locations, each which will store DNS records for a set amount of time determined by ttl.

##### Browser DNS caching

* modern web browsers are designed by default to cache DNS records for a set amount of time.
* when a request is made for a dns record, the browser cache is the fist location checked for the requested record.
* in chrome you can see by going to  chrome://net-internals/#dns


##### Operating system level dns caching

* The operating system level dns resolver is the second and last local stop before a DNS query leaves your machine.
* The process inside your operating system that is designed to handle this query is commonly called a "stub resolver" or DNS client
* when stub resolver gets a request from an application, it first checks its own cache to see if it has the record, if it does not, it then send DNS query out side the local network to a DNS recursive resolver inside the internet service provider.

**The recursive resolver also has additional functionality depending on the types of records it has in its cache.**

1. if the resolver does not have the **A records**, but does have the **NS records** for the authoritative name servers. it will query those name servers directly, bypassing  several steps in the DNS query.
2. If the resolver does not have the **NS records**, it will send a query to the TLD servers( .com in our case), skipping the root server.
3. In the unlikely event that the reolver does not have records pointing to TLD servers, it will then query the root servers. This event typically occurs after a dNS cache has been purged.


_____
##### References
1.

