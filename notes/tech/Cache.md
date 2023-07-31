# Cache
Created: 2022-05-16 20:24
Tags: 
____

#### Intro

```ad-note
title: INTRODUCTION
![[cache-0.png]]

1. __Take advantage of the locality of reference principle__
	->recently requested data is likely to be requested again.
2. __Exist at all levels in architecture__

```


1. __Precalculating results__
	-> e.g.  number of visits from each referring domain for the previous day
2. __Pre-generating expensive indexes__
	-> e.g.   suggested stores base on a users's click history 
3. __Storing copies__
	-> e.g.  Memcache instead of Postgresql
	

###### Caches in different layers
1. You have layer 1 cache memory which is the CPU cache memory
2. Then you have payer 2 cache memory 
3. Regular RAM
4. In Operating systems
5. Web Browser

[[HTTP Cache Headers]]

some keywords: __cache hit, cache miss__

#### Type of Cache

```ad-note
title: Application Server Cache

![[cache-1.png]]

The Problem arises when you need to sacle your system.
You will end up with a log of _cache miss_ because easch noed will be unware of the already cached request


We have to choses:
1. Distrubuted Cache 
2. Global Cache

```

```ad-note
title: Distributed Cache

__In distributed cache, each node will have a part of the whole cache space, and then usin the consistent haching function each request can be routed to where the cache could be found__

![[cache-2.png]]

```

```ad-note
title: Global Cache

__You will have a single cache space and all the nodes use the single space.
Every request will go to this single cache space__

![[cache-3.png]]

There are two kinds of the global cahce.

1. First, when a ache request is not found in the gloabl cache, it's responsibleity of the cache to find out the missing piesce of data drom anywhere underluing the sotore(databases, dist, etc)

2.Second, if the request comes and the cache doesn't find the data the requesting node will directly communicate with the DB or the server to fetch the requested data.





```


```ad-note
title: The CDN :))

[[CDN]] is used where a large amount of static content is server bu the website.
This can be HTML file, CSS file pictures, videos, etc
First, request ask the CDN for data,if it doesn't exist then the CDN will query the backed servers and the cache it locally


```


#### Cache Invalidation

 

[[Client Caching]]
[[CDN Caching]]
[[Webserver Caching]]
	-> can serve static and dynamic content directly.
	-> Webserver can also cache requests, returning responses without having to contact application servers.
[[Database Caching]]
	-> Databases usually includes some level of caching in a default configuration, optimized for a generic usage.
[[Application Caching]]
	-> in-memory caches such as [[memcached]] and [[REDIS]]are key-values stores between your application and your data storage.
	-> Since the data in held in RAM, it is much faster than typical databases where data is stored on DISK.
	->


_____
##### References
1.

