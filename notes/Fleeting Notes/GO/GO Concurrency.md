# GO Concurrency
Created: 2022-10-28 22:14
Tags: 
____
#### Introduction


* How should you think about concurrency?
	* when I'm solving a problem, especially if it's a new one, I don't initially think about whether concurrency is a good fit or not
	* I look for a sequential  solution first and make sure that it's working.
	* After readability and technical reviews, I will begin to ask the question if concurrency is reasonable and practical.
	* Sometimes it's obvious that concurrency is a good fit and other times it's not so clear.
	* For problem in front of you, __it has to be obvious that out of order execution would add value__.(value=enough of performance gain vs complexity cost)
	* Depending on your problem, out of execution may not be possible or even make sense.

#### Terminology
* __Work__
	* A set of instructions to be executed for a running application.
* __Thread__
	* A path of execution that is scheduled and performed.
	* Thread are responsible for the execution of instructions on the hardware.
* __Thread State__
	* A thread can be in one of three state
		* `Running`
			* means the thread is executing its assigned instructions on the hardware by have a `G`(`goroutine` )placed on The `M`(operating system thread).
		* `Runnable`
			* means the thread wants time on the hardware to execute its assigned instructions and is sitting in a __run queue__.
		* `Waiting`
			*  means the thread is waiting for something before it can resume its work.
* __Concurrency__
	* This means undefined out of order execution
	*  A work can be down concurrently when the order the work is executed in doesn't matter, as long as all the work is completed.
* __Parallelism__
	* The doing things at once. 
	* For this to be an option, I need the  ability to physically execute two or more operation system threads at the same time on the hardware.
* __CPU Bound Work__
	* This is work that does not cause the thread to naturally move into waiting state.
	* Calculating fibonacci numbers would be considered CPU-Bound work.
* __I/O Bound Work__
	* This is work that doers cause the thread to naturally move into a waiting state.
	* Fetching data from different URLs would be considered I/O=-Bound Work.
* __Synchronization__
	* When two or more `Goroutines` will need to access the same memory location potentially at the same time, they need to be synchronized and take turns.
* __Orchestration__
	* When two or more `Goroutines` need to signal each other, with or without data, orchestration is the mechanic required.
	* If orchestration doesn't take place, guarantees about concurrent work being performed and completed will be missed.

#### Concurrency vs Parallelism
* It is important to understand that concurrency __is not the same as__ parallelism.
	* Parallelism means executing two or more instructions at the same time.
		* It is only possible when you have 
			* at least two OS thread 
			* at least two hardware threads available to you 
			* at least two `goroutines`.


```ad-quote
concurrency is property of a code but parallelism is a property of execution.
```


### Go scheduler


### Patterns

### OS scheduler
###



#### How `goroutines` works 

![[go-conc-0.png]]

P1, P2 -> logical proccess

M - machine
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
		* A `Goroutine` that needs to read a file would be I/O-Bound

* With  `CPU-Bound` workloads you need parallelism to leverage concurrency
	* A single OS/hardware thread handling multiple `Goroutines` is not efficient since the `Goroutines` are not moving in and out of waiting states as part of their workload
* With `I/O-Bound` workloads you don't need parallelism to use concurrency 
	* A single OS/hardware thread can handle multiple `Goroutines` with efficiency since the `Goroutines` are naturally moving in and out of waiting states as part of their workload

-> To Few `Goroutines` and you have more idle time
-> Too many `Goroutines` and you have  move context switch latency time

_____
##### References
1.

