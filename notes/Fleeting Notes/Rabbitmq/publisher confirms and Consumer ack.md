
# publisher confirms and Consumer ack
Created: 2023-01-05 21:43
Tags: 
____

### Consumer ( delivery Acknowledgement)

When rabbit delivers a message to a consumer, it needs to know when to consider the message to be successfully sent.
What kind of logic is optimal depends on the system. 
it is therefore primarily an application decision


### manual Acknowledgement mode
-> `basic.ack`: is used for positive ack
	simply instruct rabbit to record a message as delivered and can be discarded
-> `basic.nack` is used for negative ack
-> `basic.reject`
	suggest that a delivery wasn't processed but still should be delivered

in negative acks, deliveries can be discarded or dead-lettered or requeued by the broker.
-> the behaviour is contrilled by the requeue field,

#### Automatic acknowledge mode
1. In automatic ack mode, a message considered to be successfully delivered immediately after it is sent.
2. This mode trades off higher throughput for reduced safety of delivery and consumer processing.
3. this mode is often referred to as "fire-and-forget"

publisher confirm

-> for persistent messages routed to durable queues  means persisting to disk
-> for quorum queues, this means that a quorum replicas have accepted and confirmed the message to the elected leader

_____
##### References
1.

