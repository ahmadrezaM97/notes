# Profile Go Applications
Created: 2023-04-22 20:39
Tags: 
____
### time
```
time -v ./served
```

```
go run -gcflags -m main.go
```


####  Garbage Collection Semantics


1. The heap is not an Entity
2. There is no linear containment of memory that defines the "Heap"
3. Any memory reserved for application use the process space is available for heap memory allocation.
4. Where any given heap memory allocation is virtually or physically stored is not relevant to our model

1. Tracking memory allocations in heap memory
2. Releasing allocations that are no longer needed
3. Keeping allocations that are still in-use

__As of version 1.12, Go uses a non-generational, concurrent, tri-color, mark and sweep collector.__

phases
1. collection Phases
	1. __Mark setup - Stop the world__
		1. Write Barrier
			1. to make sure concurrent activity is safe
		2. when a collection starts, the first activity that must be performed is turning on the Write Barrier
		3. In order to turn the Writer Barrier on, every application `goroutine` running must be stopped.
	2. __Marking - Concurrent__
		1. inspect the stacks to find root pointers to the heap
		2. traverse the heap graph from those root points
		3. Mark values on the heap that are still in-use
		4. collection takes 25% of the available CPU capacity for itself
		5. make assists some times spin up
	3. __Mark Termination - Stop the work__
		1. Turn the Write Barrier off
		2. various clean up tasks are performed
		3. Next collection goal is calculated

__Sweeping___
* Occurs when Goroutines attempt to allocate new heap memory
* The latency of Sweeping is added to the cost of performing an allocation, not GC








_____
##### References
1.

