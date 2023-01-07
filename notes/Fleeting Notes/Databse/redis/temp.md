# temp
Created: 2023-01-07 22:33
Tags: 
____


#### redis master slave

![[redis-master-slave.png]]

* There will be only one master with multiple slaves for replication
* All write goes to master, which creates more load on the master
* IF the master goes down, the whole architecture is prone to SPOF(single point of failure)
* Master Slave architecture does not help in scaling when your use base grows.




_____
##### References
1.

