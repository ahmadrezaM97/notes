# Go Time
Created: 2022-12-24 14:07
Tags: 
____

#### time.Ticker

```go
type Ticker struct {
	C <- chan Time
}
```
 
 a Ticker holds a channel that delivers "tick" of a clock

```go
// d must be greater than zero, otherwise it will panic
func NewTicker(d Duration) *Ticker
```

```go
func (t *Ticker)Reset(d duration)
```
Reset stops a ticker and resets its period to the specified duration. The next tick will arrive after the new period elapses.
must d > 0

```go
func (t *Ticker)Stop()
```
Stop turns off a ticker. After Stop, no more ticks will be sent.
```ad-danger
Stop doesn't close the channel, to prevent a concurrent goroutine reading from the channel from seeinig an erroneous "tick"
```

```go
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)

func main() {
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	t := time.NewTicker(1 * time.Second)
	defer t.Stop()

	var wg sync.WaitGroup

	time.AfterFunc(
		3*time.Second,
		func() {
			t.Stop()
		},
	)

	wg.Add(1)
	go func() {
		defer wg.Done()
		for {
			select {
			case p := <-t.C:
				fmt.Println("SHIT", p)
			case <-ctx.Done():
				fmt.Println(ctx.Err())
				return
			}
		}
	}()

	wg.Wait()
}

```

#### time.Timer

The Timer type represents a single event. When the Timer expires, the current time will be sent on C, unless the Timer created by `AfterFunc`.
A timer must be created with `NewTimer` or `AfterFunc`

```go
type Timer struct {
	C <- chan time
	// contains filtered or unexported fields
}
```

`AfterFunc` waits for the duration to elapse and then calls f in its own goroutine.
It returns a Timer that can be used to cancel the call using its `Stop` method.

```go
func AfterFunc(d Duration, f func()) *Timer
```



### `time.After(1*time.Second)`

```go
func After(d Duration) <-chan Time {
	return NewTimer(d).C
}
```

After wait for the duration to elapse and then sends the current time on the returned channel.

It is equivalent to `NewTimer(d).C`

```ad-danger
The underlying `Timer` is not recovered by the garbage collector until the timer fires.

if efficincy is a concern, use `NewTimer` instead and call `Timer.Stop` if the timer is not longer needed.
```


### Time

```go 
type Time struct {
	// contains filtered or unexported fileds
}
```

A Time represents an instant in time with nanosecond precision.
Programs using times should typically store and pass them as values, not pointer. That is, time variables and struct fields should be of type `time.Time` not `*time.Time`



_____
##### References
1.

