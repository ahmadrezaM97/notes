# OS Scheduler
Created: 2022-03-31 09:36
Tags: #todo 
____

##### Introduction

- You program is just a __series of machine instructions__ that need to be __executed__ one other __sequentially__.
	- To make that happen, the operating system used the concept of __Thread__
	- __Thread is a path of execution.__
- Every __program__ you run create a __Process__ and each Process is given __an Initial Thread__.
- __Threads__ have then ability __to create more Threads__.
- All these different Threads __run independently __ of each other.
- __Scheduling decisions are made at the Thread level, not process level.__
- Threads also __maintain__ their __own state__ to allow for the safe, local and independent execution of their instructions.


The __OS scheduler__ 
- is responsible for making sure __cores are not idle__ if there are Threads that can be execution.
- It must also create the illusion that all the Threads that can execute are executing at the same time.
- need to run Threads with a   __higher priority over lower__  priority Threads
	- However, Threads with a lower priority can't be starved of execution time.
- Minimize s__cheduling latencies as much as possible __by making quick and smart decisions.


Thread States
- Waiting
	- waiting for hardware (__disk, network__)
	- the operating system __system calls__
	- synchronization calls ( __atomic, mutexes__ )
	- There type of latencies are a root cause for bad performance.
- Runnable
- Executing


Type of Words
- CPU-Bound
- IO-Bound
	- this is work that causes Threads to enter into WAITING states
	- consist in 
		- requesting access to a resource over the Network
		- making system calls into the operation system
	- synchronization events (mutexes, atomix) -> that cause the Thread to waist as part of the category


[[Cache coherence problem]]

[continue ](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part1.html)
_____
##### Refrences
1.

