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

#### panic recover
#### Best Practices
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

