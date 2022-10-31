# GO Concurrency
Created: 2022-10-28 22:14
Tags: 
____
#### Introduction

```ad-note
when I'm solving a problem, especially if it's a new one, I don't initially think about whether concurrency is a good fit or not
	* I look for a sequential  solution first and make sure that it's working.
	* After readability and technical reviews, I will begin to ask the question if concurrency is reasonable and practical.
	* Sometimes it's obvious that concurrency is a good fit and other times it's not so clear
* What is concurrency 
	* For problem in front of you, it has to be obvious that out of order execution would add `value`
	* when I say `value`, I mean add enough of performance gain fr the complexity cost
		* Depending on your problem, out of execution may not be possible or even make sense.
```

#### Concurrency vs Parallelism
* It is important to understand that concurrency is not the same as parallelism
	* Parallelism means executing two or more instructions at the same time.
		* It is only possible when you have at least two OS thread and two hardwarethreads available to you and you have at least two goroutines.


```ad-quote
concurrency is property of a code but parallelism is a property of execution.
```


#### Visualize 

![[go-conc-0.png]]

P1, P2 -> logical proccess

M -> OS Thread
Core -> hardware thread

G1, G2 -> Goroutine

```ad-warning
* Sometimse leveraging concurrency without parallelism can actually slow down your throughput.

* sometimes leveraging concurrency with parallelism doesn't give you a bigger perfrmance gain than you might otherwise thin you can achieve.

```

#### Understanding your problem is a great place to start
* what kind of work load do you have?
	* CPU-Bound
		* This is a workload that never create a situation where Goroutines naturally move in and out of waiting state
		* constantly making calculations
			* calculating Pi to the Nth digit
	* I/O-Bound
		* This is a work that consists in 
			* requesting access to a resource over the network
			* making system calls into the operating system
			* waiting for an event to occur
		* A Goroutine that needs to read a file would be I/O-Bound

* With  `CPU-Bound` workloads you need parallelism to leverage concurrency
	* A single OS/hardware thread handling multiple Goroutines is not efficient since the Goroutines are not moving in and out of waiting states as part of their workload
* With `I/O-Bound` workloads you don't need parallelism to use concurrency 
	* A single OS/hardware thread can handle multiple Goroutines with efficiency since the Goroutines are naturally moving in and out of waiting states as part of their workload

-> To Few Goroutines and you have more idle time
-> Too many Goroutines and you have  move context switch latency time

_____
##### References
1.

