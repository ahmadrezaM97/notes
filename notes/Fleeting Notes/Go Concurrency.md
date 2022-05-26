# Go Concurrency
Created: 2022-04-15 05:48
Tags: 
____

-> Go was the first a major language build to take advantage of dual core cpu

##### Parallelism vs Concurrency
Parallelism is a property of execution. 
-> If you have on cpu , you wouldn't  get parallel execution.
[good talk](https://www.youtube.com/watch?v=oV9rvDllKEg)


#### BASIC
```go

log.Info(
	"GOOS: ",
	runtime.GOOS,
	" GOARCH: ",
	runtime.GOARCH,
	" NumCPU: ",
	runtime.NumCPU(),
	" NumGoroutine: ",
	runtime.NumGoroutine(),

)

```


#### sync.WaitGroup{}
->A WaitGroup waits for a collection of goroutines to finish.
->  A WaitGroup must not be copied after first use.

```go
wg := sync.WaitGroup{}
for i := 0; i < 10; i++ {
	wg.Add(1)
	go func(i int) {
		defer wg.Done()
		t := time.Duration(10)
		time.Sleep(time.Second * t)
		log.Info("End -> ", t)
	}(i)
}
wg.Wait()

```


[[CSP]]

synchronization primitives

#### [[RACECONDITION]]

```go 

// Racecondition - sample

type counter struct {
	cnt int
}

func (c *counter) inc() {
	v := c.cnt
	time.Sleep(time.Second)
	v = v + 1
	// runtime.Gosched()
	c.cnt = v
}

func main() {
	c := counter{cnt: 0}
	var wg sync.WaitGroup
	for i := 0; i < 10000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			c.inc()
		}()
	}
	wg.Wait()
	fmt.Println(c.cnt)
}

```

-> As a high-level approach, using channels to control access makes it easier to write clear, correct programs.


#### Mutex
-> TO PREVENT RACE CONDITION 

```go
type counter struct {
	cnt  int
	lock sync.Mutex
}

func (c *counter) inc() {
	c.lock.Lock()
	defer c.lock.Unlock()

	v := c.cnt
	time.Sleep(time.Microsecond)
	v = v + 1
	// runtime.Gosched()
	c.cnt = v
}

func main() {
	c := counter{cnt: 0}
	var wg sync.WaitGroup
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			c.inc()
		}()
	}
	wg.Wait()
	fmt.Println(c.cnt)
}

```


### ATOMIC
Package atomic provides low-level atomic memory primitives useful for implementing synchronization algorithms.

These functions require great care to be used correctly. Except for special, low-level applications, synchronization is better done with channels or the facilities of the sync package. Share memory by communicating; don't communicate by sharing memory.


#### [[GO Channels]]


_____
##### References
1.

