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

![[dns-2.png]]


![[dns-1.png]]




A Domain Name System (DNS) translates a domain name such as www.example.com to an IP address.

DNS is hierarchical, with a few authoritative servers at the top level.  Your router or ISP provides information about which DNS server(s) to contact when doing a lookup.  Lower level DNS servers cache mappings, which could become stale due to DNS propagation delays.  DNS results can also be cached by your browser or OS for a certain period of time, determined by the [time to live (TTL)](https://en.wikipedia.org/wiki/Time_to_live).

* **NS record (name server)** - Specifies the DNS servers for your domain/subdomain.
* **MX record (mail exchange)** - Specifies the mail servers for accepting messages.
* **A record (address)** - Points a name to an IP address.
* **CNAME (canonical)** - Points a name to another name or `CNAME` (example.com to www.example.com) or to an `A` record.

Services such as [CloudFlare](https://www.cloudflare.com/dns/) and [Route 53](https://aws.amazon.com/route53/) provide managed DNS services.  Some DNS services can route traffic through various methods:

* [Weighted round robin](https://www.jscape.com/blog/load-balancing-algorithms)
    * Prevent traffic from going to servers under maintenance
    * Balance between varying cluster sizes
    * A/B testing
* [Latency-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-latency)
* [Geolocation-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-geo)

### Disadvantage(s): DNS

* Accessing a DNS server introduces a slight delay, although mitigated by caching described above.
* DNS server management could be complex and is generally managed by [governments, ISPs, and large companies](http://superuser.com/questions/472695/who-controls-the-dns-servers/472729).
* DNS services have recently come under [DDoS attack](http://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/), preventing users from accessing websites such as Twitter without knowing Twitter's IP address(es).

### Source(s) and further reading

* [DNS architecture](https://technet.microsoft.com/en-us/library/dd197427(v=ws.10).aspx)
* [Wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)
* [DNS articles](https://support.dnsimple.com/categories/dns/)



_____
##### References
1.

