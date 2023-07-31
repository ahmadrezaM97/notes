# Reflection
Created: 2022-10-18 21:39
Tags: 
____

* *`interface{}` says nothing
* we can extract a specific type with a type assertion (a/k/a `downcasting`)

`value.(T)`

``` go
	var w io.Writer = os.Stdout

	f := w.(*os.File) // success : f == os.Stdout
	c := w.(*bytes.Buffer) // panic: interface holds *osFiles, not *byte.Buffer
```

we can use the two-result version, we can avoid `panic`

```go
	var w io.Writer = os.Stdout
	f, ok := w.(*os.File)
	
```


`a := arg.(type)`


_____
##### References
1.

