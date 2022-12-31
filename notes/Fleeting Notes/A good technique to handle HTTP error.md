Created: 2022-12-23 18:32
Tags: 
____
```go

// a good technique for http error handling


type appError struct {
	Error error
	Message string
	Code int
}

type appHandler func(http.ResponseWriter, *http.Request) *appError



func (fn appHandler) ServeHTTP(w http.ResponseWriter, r *http.Request){
	if err := fn(w,r); err != nil {
		http.Error(w, e.MEssage, e.Code)
	}
}

func viewHandler(w http.ResponseWriter, r *http.Request) * appError {
	if somethinghappend {
		return &appError{err, "can't display", 500}
	}

	return nil
}

http.Handle("/view", appHanler(viewHandler))


```

https://go.dev/blog/error-handling-and-go
_____
##### References
1.

