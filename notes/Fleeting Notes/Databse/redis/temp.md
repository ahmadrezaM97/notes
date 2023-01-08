# temp
Created: 2023-01-07 22:33
Tags: 
____
https://www.alibabacloud.com/blog/high-concurrency-practices-of-redis-snap-up-system_597858

https://redis.io/docs/manual/patterns/distributed-locks/

https://redis.com/ebook/part-2-core-concepts/chapter-6-application-components-in-redis/6-2-distributed-locking/6-2-3-building-a-lock-in-redis/

https://architecturenotes.co/redis/

redis cluster 
https://medium.com/@bb8s/deep-dive-into-redis-cluster-sharding-algorithms-and-architecture-a093a92e97af


https://medium.com/@bb8s/how-redis-cluster-achieves-high-availability-and-data-persistence-8cdc899764e8


deep dive into redis replication

https://www.youtube.com/watch?v=esbRryo0Ty8



redis perssistence AOF and RDB + docekr
https://www.youtube.com/watch?v=1pfvz24BAUs


// 10 things about redis
https://www.youtube.com/watch?v=y6k9dRHDaA0

#### redis master slave

![[redis-master-slave.png]]

* There will be only one master with multiple slaves for replication
* All write goes to master, which creates more load on the master
* IF the master goes down, the whole architecture is prone to SPOF(single point of failure)
* Master Slave architecture does not help in scaling when your use base grows.




_____
##### References
1.

