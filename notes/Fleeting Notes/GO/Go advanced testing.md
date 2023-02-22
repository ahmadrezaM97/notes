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

#### Repeat yourself

1. Localized logic is more important than test lines of code
2. When a test fails, you very often don't remember the details of the test.It is very cumbersome to have logic spread across multiple call site.
3. Limit helpers to very reused logic don't that doesn't fail often or fail all at once.
4. Helpers only help the person who know they exist and what they do.


#### Packages/ Functions

1. break down functionality into packages/ function judiciously
	1. Do not overdo it. Do it where it makes sense.
2. Doing this correctly will aid testing while also improving organization over-doing it will complicated testing and readability
3. Qualitative, but practice will make perfect
4. Unless the function is _extremely_ complex, we try to test only the exported functions, the exported API.
5. We treat unexported functions/structs as implementation details
	1. they are a means to an end.
	2. As long as we test the end and it behaves within spec, the means don't matter.
6. Some people take this too far and choose to only integration/ acceptance test, the ultimate "test the end, ignore the means". We disagree with this approach.
7. 

_____
##### References
1.

