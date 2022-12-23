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


[[ A good technique to handle HTTP error ]]

[[Nil Error Mistake]]


#### panic recover
#### Best Practices
#### Wrap Error
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

