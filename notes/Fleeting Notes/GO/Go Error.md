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

[[A good technique to handle HTTP error ]]
[[Nil Error Mistake]]

#### Wrap Error

Since Go 1.13, the %w directive allow us to wrap errors conveniently.
In general, the two main use cases for error wrapping are 
	1. Adding additional context to an error
	2. making an error as specific error

case study:
we receive a request from a specific user to access a database resource, but we get a "permission denied" error during the query.
for debugging purposes, if the error is eventually logged, we want to add extra context. in this case, we can wrap the error to indicate who the user is and what resource is being accessed.

![[go-error-0.png]]


```go
if err != nil {
	return fmt.Errorf("bar failed: %w", err)
}
```

![[go-error-2.png]]

Options:
1. returning error directly
	1. without extra context
	2. we can not mark error
	3. source error is available
2. Custom error type
	1. extra context is possible
	2. we can mark error
	3. source error availability is possible
3.  fmt.Error with %w
	1. with extra context 
	2. we can not mark error
	3. source error is available
4. fmt.Error with %v
	1. with extra context
	2. we can not mark error
	3. source error is not avaible

### Do not handle an error twice
logging an error is handling an error. Hence, we should either log or return an error.


### Please be careful when you wanna ignore erorr

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

#### defer panic recover
https://go.dev/blog/defer-panic-and-recover

Defer statements allow us to think about closing each file right after opening it, guaranteeing that, regardless of the number of return statements in the function, the files will be closed.
```ad-danger 
1. A deffered function's arguments are evaluated when the defer statement is evalueted
2. Deffered function calls are executed in Last in First Out order after the surreounding function returns.
3. Deferred functions may read and assign to the returning function's named return values
```

__Panic__ is a built-in function that stops the ordinary flow of control and begins panicking. when the function F calls panic, execution of F stops, any deferred functions in F are executed normally and then F returns to it's caller. To the caller, F then behaves like a call to panic.
The process continues up the stack until all functions in the current have returned
panic can be initiated by invoking panic directly. they can also be caused by runtime errors, such as out-of-bounds array accesses.

__Recover__ is built-int function that regains control of a panicking Goroutine. Recover is only useful inside deferred functions
During normal execution, a call to recover will return nil and have not other effect.
If the current goroutine i panicking, a call to recover will capture the value given to panic and resume normal execution.

#TODO 
add example codes


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

#### Custom Error

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



_____
##### References
1.

