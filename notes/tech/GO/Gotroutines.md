# Gotroutines
Created: 2022-03-31 07:39
Tags: 
____
#### Goroutines

A goroutine is a unit of independent execution (coroutine)

```ad-tip
-> It's has its own call stack, which grows ans shrinks as required.
-> It's very cheap. It's practival to have thousands, even hundres of thousands of goroutines.
-> It's not a thread.
	-> there might be only one thread in a program with thousands of goroutines
	
Goroutines are multiplexed dynamically onto threads as needed to keep all the goroutines running.


```


It's easy to start a goroutine.
The trick is knowing how the goroutine will stop:
- you have a well-defined loop termination condition
- you signal completion through a channel or [[Context - GO]]
- you let it run until the program stops

```ad-danger 
title: YOU MUST
Makesure it DOES NOT get BLOCKED by mistake.
```
[[Goroutine Leak]]


> Goroutine is not a [[Thread- OS ]]




_____
##### Refrences
1.[ Go Class: 23 CSP, Goroutines, and Channels](https://www.youtube.com/watch?v=zJd7Dvg3XCk)

