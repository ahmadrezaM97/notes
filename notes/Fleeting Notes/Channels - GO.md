# Channels - GO
Created: 2022-03-31 05:54
Tags: 
____

#### [[Channels - GO]]




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




_____
##### Refrences
1.[ Go Class: 23 CSP, Goroutines, and Channels](https://www.youtube.com/watch?v=zJd7Dvg3XCk)



