# temp
Created: 2023-01-07 22:33
Tags: 
____
https://architecturenotes.co/redis/

redis cluster 
https://medium.com/@bb8s/deep-dive-into-redis-cluster-sharding-algorithms-and-architecture-a093a92e97af



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

