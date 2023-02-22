# Go advanced testing
Created: 2023-02-21 21:23
Tags: 
____
todo:
1. fuztest
2. linter
3. %v and %s with error

### Global state

1. Avoid it as much as possible.
2. Instead of global state, try to make whatever is global a configuration option using global state as the default, allowing test to modify it.
3. If necessary, make global state a var so it can be modified. The is a last case scenario, thought.

### Test Helpers

1. never return errors. Pass in `*testing.T` and fail.
2. By not returning errors, usage is much prettier since error checking is gone.
3. Used to make tests clear on what they're testing vs what is boilerplate
4. Call `t.Helper()` for cleaner failure output(Go 1.9)
5. Returning a `func()` for cleanup is an elegant way hide that
	1. The` func()` is a closure that can have access to `*testing.T` to also fail
6. Helpers only help the person who knows they exist and what they do.
7. Localized logic is more important than test lines of code.
8. 

```go
func testTempfile(t *testing.T) string {
	t.Helper()
	tf, err := ioutl.TempFile("","test")
	if err != nil {
		t.Fatelf("err: %v", err)
	}
	tf.Close()

	// return cleanup value
	return tf.Name(), func() { os.Remove(tf.Name())}
}


func TestThing(t *testing.T) {
	tf, tfclose := testTemptFile(t)
	defer tfclose()
	
}
```

```go
t.Helper() // make stack trace better. since go:1.9
```

https://www.youtube.com/watch?v=8hQG7QlcLBk


_____
##### References
1.

