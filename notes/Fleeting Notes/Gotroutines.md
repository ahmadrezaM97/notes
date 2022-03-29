# Gotroutines
Created: 2022-03-31 07:39
Tags: 
____
#### Goroutines

A goroutine is a unit of independent execution (coroutine)

It's easy to start a goroutine.
The trick is knowing how the goroutine will stop:
- you have a well-defined loop termination condition
- you signal completion through a channel or [[Context - GO]]
- you let it run until the program stops

```ad-danger 
title: YOU MUST
Mkesure it DOES NOT get BLOCKED by mistake.
```
[[Goroutine Leak]]


> Goroutine is not a [[Thread- OS ]]




_____
##### Refrences
1.[ Go Class: 23 CSP, Goroutines, and Channels](https://www.youtube.com/watch?v=zJd7Dvg3XCk)

