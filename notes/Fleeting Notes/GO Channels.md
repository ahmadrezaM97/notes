GO Channels
Created: 2022-03-31 05:54
Tags: 
____

#### [[GO Channels]]




#### channel
A channel is like a one-way socket or a Unix pipe

It is a method of synchronization as well as communication

writes always happens before a receive(read)

It's also a vehicle for transferring ownership of data, so that only one groutine at a time is writing the data.

```ad-quote
title: Rob Pike
Don't communicate by shareing memory; instead, share memoery by communicating.
```


>restrict a type of channel with <- or ->


#### Signaling Semantics

- Orchestration
	- waitgroup
- Synchronization

___Signaling___ behavior of channels

a goroutine send signal to an other goroutine

send/reveice

Guarantee OF Delivery
- if we want that guarantee -> unbuffered channel s
- if we don't want -> buffered channels


channel state
![[channel-go-state.png]]
>yo do not have to close a channel, closing a channel is state change not a memory cleanup situation



_____
##### Refrences
1.[ Go Class: 23 CSP, Goroutines, and Channels](https://www.youtube.com/watch?v=zJd7Dvg3XCk)



