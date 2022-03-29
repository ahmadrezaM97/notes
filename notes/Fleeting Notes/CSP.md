# CSP
Created: 2022-03-31 07:37
Tags: 
____

A Channel is a one-way communication pipe
have order

> multiple readers & writers can share it safely

### Communicating Sequential Processes
- each part is independent
- all they share are the channels between them
- the parts can run in parallel as the hardware allows


> [[Concurrency]] is always hard 

CSP provides a model for thinking about it that makes it __less harder__
	- Take the program apart and make the pieces talk to each other

```ad-quote
title: Andrew Gerrand:
Go doesn't force developers to embrace the asynchronous ways of event-driven programming.
That lets you write asynchronous code in synchronous style.
As people, we're much better suited to writing about things in a aynchronous style.

```




_____
##### Refrences
1.[ Go Class: 23 CSP, Goroutines, and Channels](https://www.youtube.com/watch?v=zJd7Dvg3XCk)

