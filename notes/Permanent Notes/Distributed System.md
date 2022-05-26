# Distributed System
Created: 2022-05-11 21:52
Tags: 
____

### Model in distributed system


 A system model capture our assumption about how nodes and the network behave

#### The two general problem 

![[the-two-general-problem.png]]

![[the-two-general-problem-1.png]]

![[the-two-general-problem-2.png]]



#### The Byzantine generals problem
![[the-byzantine-generals-problem-0.png]]

-> This time we assume that if a message is sent, it is always delivered correctly
-> We have more two army 
-> The challenge here is that some generals might be "traitors", It means they could send maliciously mislead and confuse the other generals

![[the-byzantine-generals-problem-1.png]]

[[The-byzantine-generals-problem-and-blockchain]]




#### Describing node and network behaviour

##### Network behaviour(e.g. message loss)
![[system-model-network.png]]

__Fair-loss__ -> __Reliable__ keep retrying and maybe [[Dedup]]
__Arbitrary link__ -> __ Fair-loss __- > TLS   (If network not blocked )

#### Node behaviour (e.g. crashes)

![[system-model-node.png]]

__Crash-Stop__ -> you burn your phone :D
__Crash-recovery__ -> Typical failure
__Byzantine__ ->

####  Timing behaviour (e.g. latency)

![[system-model-tiem.png]]





### Fault tolerance 

