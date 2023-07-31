# Go Error
Created: 2022-12-11 18:25
Tags: 
____
### Introduction

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

[[Defer Panic Recover]]
[[multi error]]
[[call stack in golang error]]


### Wrap


#### Wrapping errors with %w

It is common to use the `fmt.Errorf` function to add additional information to an error
```go
if err != nil {
	return fmt.Errorf("decompress %v: %v", name, err)
}
```

1. since `Go1.13`, the `fmt.Errof` function supports a new `%w` verb
2. when this verb is present, the error returned by `fmt.Errorf`__ will have and `Unwrap` method__ returning the argument of %w, which must be an error.
3. __in all other ways, %w is identical to %v__


#### Examining errors with Is and As

##### `errors.Is`

```go
func Is(err, target error) bool
```

__Is reports whether any error in err's chain matches target.
The chain consists of err it self followed by the sequence of errors obtained by repeatedly calling Unwrap__

```go
if errors.Is(err, ErrPermission) {
    // err, or some error that it wraps, is a permission problem
}
```

```go
// Good:
func handlePet(...) {
    switch err := process(an); {
    case errors.Is(err, ErrDuplicate):
        return fmt.Errorf("feed %q: %v", an, err)
    case errors.Is(err, ErrMarsupial):
        // ...
    }
}
```

##### `errors.As`

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
	return fmt.Errorf("do custom thing: %w", &MyError{msg: "Random"})

}

func main() {
	if err := DoSampleError(); err != nil {
		fmt.Println(err)
		fmt.Println(errors.Is(err, ErrSample))
	}

	if err := DoCustomError(); err != nil {
		var p *MyError
		fmt.Println(
			err,
			errors.As(err, &p),
		)
	}
}

```

#### Whether to wrap

when adding additional context to an error, either `fmt.Errorf` or by implementing a custom type, you need to decide whether the new error should wrap the original.

__There is no single answer to this question it depends on the context in which the new error is created__

Wrap an error to expose it to callers
Do not wrap and error when doing so would expose implementation details.



### Mistakes
[[A good technique to handle HTTP error ]]
[[Nil Error Mistake]]


#### Don't miss defer error

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

	return errors.New("ooo")
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
err := fmt.Errorf("Something bad happend.")

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


###Stack traces and the errors packages


#### adding information

```ad-note
the programmer should ensure there’s sufficient information in the error without adding duplicate or irrelevant detail. 

If you’re unsure, try triggering the error condition during development: that’s a good way to assess what the observers of the error (either humans or code) will end up with.
```

BAD

```go
// Bad:
if err := os.Open("settings.txt"); err != nil {
    return fmt.Errorf("could not open settings.txt: %w", err)
}

// Output:
//
// could not open settings.txt: open settings.txt: no such file or directory
```


GOOD

```go
// Good:
if err := os.Open("settings.txt"); err != nil {
    // We convey the significance of this error to us. Note that the current
    // function might perform more than one file operation that can fail, so
    // these annotations can also serve to disambiguate to the caller what went
    // wrong.
    return fmt.Errorf("launch codes unavailable: %v", err)
}

// Output:
//
// launch codes unavailable: open settings.txt: no such file or directory
```


```ad-warning
title: wrapping
It is best to avoid using `%w` unless you also document (and have tests that  
validate) the underlying errors that you expose. If you do not expect your  
caller to call `errors.Unwrap`, `errors.Is` and so on, don’t bother with `%w`.
```

```go

err2 := fmt.Errorf("err2: %w", err1)
```

https://google.github.io/styleguide/go/best-practices.html#error-handling
https://google.github.io/styleguide/go/decisions#errors


#### best practice

Do not handle an error twice
logging an error is handling an error. Hence, we should either log or return an error.


in Go error are just values, they are created by code and consumed by code
	1. Converted into diagnostic information for display to humans.
	2. Used by the maintainer
	3. interpreted by an end user

Error messages also show up across a variety of different surfaces including log messages, error dumps, and rendered UIs


Code that process ( consumers or produces) error should do so deliberately
it can be tempting to ignore, or blindly propagate an error 
it is always worth considering whether the current function is the call frame is positioned to handle error most effectively.

1. when creating an error value, decide whether to give it any structure
2. when handling an error, consider adding information that you have by the caller and/or callee might not.

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

###### Please be careful when you wanna ignore error
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


_____
##### References
1.

