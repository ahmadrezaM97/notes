# Go Error
Created: 2022-12-11 18:25
Tags: 
____
#### introduction

```go
if err != nil {
	return err
}
```

```ad-note

Errors are values
Values can be programmedm and since errors are value, errors can
be programmed.

```

```go
type error interface {
	Error() string
}


//  a trivial implementation of error
type errorString struct {
	s string
}

func (e *errorString) Error() string {
	return e.s
}
```


errors.Is

__Is reports whether any error in err's chain matches target.
The chain consists of err it self followed by the sequence of errors obtained by repeatedly calling Unwrap__


errors.As

__As finds the first error in err's chain that matches target

The chain consists of err it self followed by the sequence of errors obtained by repeatedly calling Unwrap 

An error matches target if the error's concrete value is assignable to the value pointed to by target, or if the error has a method As(interface{}) bool such that As(target) returns true. In the latter case, the As method is responsible for setting target.


```go
package main

import (
	"errors"
	"fmt"
)

var (
	ErrSample = errors.New("sample")
)

func DoSampleError() error {

	f := func() error {
		return fmt.Errorf("first: %w", ErrSample)
	}

	if err := f(); err != nil {
		return fmt.Errorf("second: %w", err)
	}

	return nil

}

type MyError struct {
	msg string
}

func (c *MyError) Error() string {
	return c.msg
}

func DoCustomError() error {
	return &MyError{msg: "Random"}

}

func main() {
	if err := DoSampleError(); err != nil {
		fmt.Println(err)
		fmt.Println(errors.Is(err, ErrSample))
	}

	if err := DoCustomError(); err != nil {
		var p *MyError
		fmt.Println(
			errors.As(err, &p),
		)
	}
}

```


### Mistakes
[[A good technique to handle HTTP error ]]
[[Nil Error Mistake]]


### Do not handle an error twice
logging an error is handling an error. Hence, we should either log or return an error.


### Please be careful when you wanna ignore error

In some cases, we may want to ignore an error returned by a function.

```go

func f(){
	// ...
	notify()
}

func notify() error {
	// ...
}
```

for a maintainability perspective, the code can lead to some issues
Do this

```go
func f(){
	// at-most once delivery.
	// Hence, it's accepted to miss some of them in case of errors
	_ = notify()
}
```


### Defer panic recover
https://go.dev/blog/defer-panic-and-recover

Defer statements allow us to think about closing each file right after opening it, guaranteeing that, regardless of the number of return statements in the function, the files will be closed.

```ad-note
title: rule 1
1. A deffered function's arguments are evaluated when the defer statement is evalueted
```
```go

package main

import "fmt"

func Do() {
	value := 0
	defer fmt.Println(value) // value is zero(0)

	value = 19
}
func main() {
	Do()
}
```

```
0
```

```ad-note 
title: rule 2
2. Deffered function calls are executed in Last in First Out order after the surreounding function returns.
```


```go
package main

import "fmt"

func Do() {
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)

	panic("panic")

	defer fmt.Println(4)
}
func main() {
	Do()
}
```

```
3
2
1
panic: panic

goroutine 1 [running]:
main.Do()
	/home/ahmadreza/Documents/personal/tamrinat/goclass/errorl/main.go:10 +0xf4
main.main()
	/home/ahmadreza/Documents/personal/tamrinat/goclass/errorl/main.go:15 +0x17
exit status 2
```


```ad-note 

3. Deferred functions may read and assign to the returning function's named return values
```
```go
package main

import (
	"errors"
	"fmt"
)

func Do() (err error) {

	defer func() {
		err = errors.New("defer error")
	}()

	err = errors.New("normal error")

	return err
}

func main() {
	if err := Do(); err != nil {
		fmt.Println(err)
	}
}
```

```
defer error
```


__Panic__ is a built-in function that stops the ordinary flow of control and begins panicking. when the function F calls panic, execution of F stops, any deferred functions in F are executed normally and then F returns to it's caller. To the caller, F then behaves like a call to panic.
The process continues up the stack until all functions in the current have returned
panic can be initiated by invoking panic directly. they can also be caused by runtime errors, such as out-of-bounds array accesses.

__Recover__ is built-int function that regains control of a panicking Goroutine. Recover is only useful inside deferred functions
During normal execution, a call to recover will return nil and have not other effect.
If the current goroutine i panicking, a call to recover will capture the value given to panic and resume normal execution.

#TODO 
add example codes

### Don't miss defer error

```go
package main

import (
	"errors"
	"fmt"
	"log"
)

func badThingsMightHappen() error {
	return errors.New("A very bad thing happened")
}

func HandleDeferError() (err error) {
	defer func() {
		deferErr := badThingsMightHappen()
		if err != nil {
			if deferErr != nil {
				// (err!=nil, deferErr!=nil) -> log deferErr and return err
				log.Printf("defer error: %v\n", deferErr)
			}
			// (err!=nil, deferErr=nil) -> return err
			return
		}
		// (err=nil, defer=?) return defer err
		err = deferErr
	}()

	return errors.New("SHIT")
}

func main() {
	fmt.Println(HandleDeferError())
}

```


#### Best Practices

1. Use `error` to signal that a function can fail. by convention, `error` is the last parameter.
2. returning a `nil` error is the idiomatic way to signal a successful operation that could otherwise fails.
	1. if a function returns an error callers must treat all non-error return values as unspecified unless explicitly documented otherwise.
	2. the non-error return values their zero values, but this cannot be assumed
3. Exported function that return errors should return them using the `error` type
	1. Concrete `error` type are susceptible to subtle bugs: a concrete `nil` pointer can get wrapped into an interface and thus become non-nil value.


Error strings
1. Error string should not be capitalized(unless beginning with an exported name, a proper noun or an acronym) and should not end with punctuation. ->because it usually apear within other context before being printed to the user

```go
// bad
err := fmt.Errorf("something bad happend.")

// good
err := fmt.Errorf("something bad happend")

```

On the other hand, the style for the full display message
	logging
	rest failure
	API response or other UI

depends, but should typically be capitalized.
```go
log.Infof("Operation aborted: %v", err)
```

https://google.github.io/styleguide/go/best-practices.html#error-handling
https://google.github.io/styleguide/go/decisions#errors


### Error handling best practice

in Go error are just values, they are created by code and consumed by code
	1. Converted into diagnostic information for display to humans.
	2. Used by the maintainer
	3. interpreted by an end user

Error messages also show up across a variety of different surfaces including log messages, error dumps, and rendered UIs


Code that process ( consumers or produces) error should do so deliberately
it can be tempting to ignore, or blindly propagate an error 
it is always worth considering whether the current function is the call frame is positioned to handle error most effectively.

1. when creating an error value, decide whether to give it any structure
2. when handling an error, consider adding information that you have byt the caller and/or callee might not.

While is is usually not appropriate to ignore an error, a reasonable exception to this is when orchestrating related operations
where often only the first error is useful.
package `errgroup` provide a convenient abstraction for a group of operations that can all fail or be canceled as a group.

[[Error group]]

```go
	ctx := context.Background()
	g, ctx := errgroup.WithContext(ctx)

	for i := 0; i < 10; i++ {
		i := i
		g.Go(func() error {
			if rand.Intn(5)%2 == 0 {
				return fmt.Errorf("random Eror")
			}
			fmt.Println(i)
			return nil
		})
	}
	g.TryGo()
	if err := g.Wait(); err != nil {
		log.Println(err)
	}
	
```

```
### Custom Error

The error type

```go 
type errorString struct {
	s string 
}

func (e *errorString) Error() string {
	return e.s
}
```

```go
type SyntaxError struct {
	msg string 
	Offset int64
}

func (e *SyntaxError) Error() string { return e.msg }
```


[[multi error]]
[[call stack in golang error]]
_____
##### References
1.

