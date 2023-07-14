# file descriptor
Created: 2023-07-14 20:18
Tags: 
____

* A number to describe an open file or I/O resource in system
	* Unique non-negative integer
* Describes resource and how it can be accessed
* When a process requests for resource
	* kernel grant access( if valid)
	* creates entry in global file table
	* provides process with location of that entry


* Each process has a file descriptor table
	* Process cannot access it,its hidden from process
	* 0, 1, 2 are special file descriptor

 ![[fd-0.png]]


* Descriptor Table
	* Per-process
	* indexed by file descriptor
	* Contains pointers to the open file table

```
ulimit -a
```



_____
##### References
1.

