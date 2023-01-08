# persistent
Created: 2023-01-09 03:06
Tags: 
____


in many cases, redis can be used as a persistent storage


Redis has built-in AOF ( append only log file) mechanism that can be enabled to persist every write operation to disk.

AOF has two modes that can be configured

1. Synchronous AOF
	1. (blocking write)
		1. every write needs to be successfully persisted to disk before returning to client
	2. Redis wouldn't be a purely "in-mem" storage
	3. Synchronous AFO ensures persistence, but comes with a __tradeoff  of high write latency__
	4. (about ~1s!!!!!!!!) search more
2. Async AOF:
	1. (non blocking write)
	2. write is non-blocking, meaning that redis can be considered as "in-mem" for clients
	3. in-mem data is periodically persisted to disk in the background
	4. This mode gives very low write latency since it is still in-mem for clients
	5. but comes with __a tradeoff of data loss__
	6. if redis node fails or restarts, then data write to this redis node but no persisted to disk yet will be permanently lost.


