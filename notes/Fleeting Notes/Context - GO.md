# Context - GO
Created: 2022-10-04 00:43
Tags: 
____
### introduction
* __In Go servers, each incoming request is handled in its own goroutine_
* request handlers often start additional goroutines to access backedns such as db or RPC services
* => the set of goroutines working on a request typically needs access to request-specific values
	* identity of the end user
	* authorization tokens
	* request deadline
	* when a request is canceled or times out -> all the gouroutines working on that request should exit quickly so the system can reclaim any resources they are using

```ad-note

The `context` package created to make it easy to pass request-scoped values, cancellation signals and deadlines across API boundaries.
```

#### Context

```go
// A Context carries a deadline, cancellation signal, and request-scoped values
// across API boundaries. Its methods are safe for simultaneous use by multiple
// goroutines.
type Context interface {
    // Done returns a channel that is closed when this Context is canceled
    // or times out.
    Done() <-chan struct{}

    // Err indicates why this context was canceled, after the Done channel
    // is closed.
    Err() error

    // Deadline returns the time when this Context will be canceled, if any.
    Deadline() (deadline time.Time, ok bool)

    // Value returns the value associated with key or nil if none.
    Value(key interface{}) interface{}
}
```


* `Done() <- chan struct{}` -> returns a channel that is closed when this context is canceled or times out
* `Err() err` -> Err indicates why this context was canceled, after Done channel is closed
* `Deadline() ( deadline time.Time, ok bool)` -> Deadline returns the time when this context will be canceled, if any
* `Value( key interface{}) interface` -> value returns the value associated with key or nil if none

#### `func Background() Context`
Background returns an empty Context
It is never canceled, has no deadline and hos no value
it is typically used in main, init , and test

#### `func WithCancel(parent Context) (ctx Context, cancel CancelFunc)`

WithCancel returns a  copy of parent show Done channel is closed as soon ass parent.Done is closed or cancel is called

```go
type CancelFunc func()
```

#### `func WithTimout( parent Context, timeout time.Duration) (Context, CancelFunc`
returns a copy of parent whose Done
1. as soon as parent.Done() is closed
2. cancel is called
3. timeout elapse

### Understand Context
We are using context for 
	1. better-controlling goroutines
	2. saving more resource with good cancelation
	3. bringing shared request-scope variable between gourotines that are working on a request

In GO, `context` is delivered layer by later with goroutine, so when an error occurs in the upper layer, and termination is required, the Gouroutine in the lower layer can get the notice and process( avoiding additional overhead)

#### background and todo
```go
var (
	background = new(emptyCtx)
	todo = new(emptyCtx)
)

func Background() Context {
	return background
}

func TODO() Context {
	return todo
}
```

`context.Background()` -> return an empty context usually in the main or main thread, to create the parent context

`context.TODO()` -> also creates an empty context and is used in the main thread but has a wider application
you can use it when you are unsure of what context to use, or when you plan to reveive a context

they are just aliases for each other with no much difference

#### Using Context

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
func WithValue(parent Context, key, val interface{}) Context
```

##### `WithCancel`
one use case for using `Withcancel` is when you launched multiple goroutine concurrently and one of them has been failed the you can cancel the context to stop others 
example:
```go 
	bctx := context.Background()
	ctx, cancel := context.WithCancel(bctx)

	wg := sync.WaitGroup{}

	wg.Add(1)
	go func(ctx context.Context) {
		defer wg.Done()
		t := time.NewTicker(time.Second)
		for {
			select {
			case <-t.C:
				fmt.Println("I'm Alive")
			case <-ctx.Done():
				fmt.Printf("you killed me because: %q\n", ctx.Err().Error())
				return
			}
		}
	}(ctx)

	time.AfterFunc(5*time.Second, func() {
		fmt.Println("I want to kill you!")
		cancel()
	})

	wg.Wait()
```

what's going on behind the scenes?
```go

func WithCancel(parent Context) (ctx Context, cancel CancelFunc) {
	if parent == nil {
		panic("cannot create context from nil parent")
	}
	c := newCancelCtx(parent)
	propagateCancel(parent, &c)
	return &c, func() { c.cancel(true, Canceled) }
}
// newCancelCtx returns an initialized cancelCtx.
func newCancelCtx(parent Context) cancelCtx {
	return cancelCtx{Context: parent}
}

// A cancelCtx can be canceled. When canceled, it also cancels any children
// that implement canceler.
type cancelCtx struct {
	Context

	mu       sync.Mutex            // protects following fields
	done     atomic.Value          // of chan struct{}, created lazily, closed by first cancel call
	children map[canceler]struct{} // set to nil by the first cancel call
	err      error                 // set to non-nil by the first cancel call
}

type cancler interface {
	cancel(removeFromParent bool, err error) Done() <-chan struct{}
}

```


#TODO: include code and complete this note

#### Best Practices
* Do not store contexts inside a struct type; instead, pass a context explicitly to each function that needs it 
* Context in your API is intended to be request scope
	* it would make sense to exist along a single database query, but wouldn't make sense to exist along a database object
* Do not pass a nil context, even if a function permits it.
	* Pass a TODO context if you are unsure about with context to use.

_____
##### References
1.

