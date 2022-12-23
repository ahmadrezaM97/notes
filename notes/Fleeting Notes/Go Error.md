# Go Error
Created: 2022-12-11 18:25
Tags: 
____
The error type

```go
type error interface {
	Error() string
}
```

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

