# Handle error gracefully
Created: 2023-04-22 19:49
Tags: 
____

>do not just check the error, handle them gracefully

#### Sentinel Error

```go
io.EOF
sysncall.ENOENT
```

value in error.Error() exist for human do not check that in code flow

__Sentinel errors become part of you public API__

create dependency between two packages


#### Error type


```go

type PathError struct {
	Op string
	Path string
	Err error
}

func (e *PathError) Error() string {
	return e.Op + " " + e.Path + ": "+ e.err.Error()
}



err := something()
switch err := err.(type){
	case nil:
		// call suncceeded, nothing to do
	case *os.PathError:
		// handle PathError specifically
	default:
		// unknown error
}

```


#### Opaque errors

```go

func fn() error {
	x, err := bar.Foo()
	if err != nil {
		return err	
	}
	// use x
}
```


```ad-quote
Assert errors for behaviour, not type.
```




_____
##### References
1.

