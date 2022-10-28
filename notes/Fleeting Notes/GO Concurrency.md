# GO Concurrency
Created: 2022-10-28 22:14
Tags: 
____
#### Introduction

```ed-note
when I'm solving a problem, especially if it's a new one, I don't initially think about whether concurrency is a good fit or not
	* I look for a sequential  solution first and make sure that it's working.
	* After readability and technical reviews, I will begin to ask the question if concurrency is reasonable and practical.
	* Sometimes it's obvious that concurrency is a good fit and other times it's not so clear
* What is concurrency 
	* For problem in front of you, it has to be obvious that out of order execution would add `value`
	* when I say `value`, I mean add enough of performance gain fr the complexity cost
		* Depending on your problem, out of execution may not be possible or even make sense.
```
* 
* It is important to understand that concurrency is not the same as parallelism
	* Parallelism means executing two or more instructions at the same time.
		* It is only possible when you have at least two OS thread and two hardware threads available to you and you have at least two goroutines.

_____
##### References
1.

