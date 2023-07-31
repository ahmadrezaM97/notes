# GO HTTP HANDLER
Created: 2022-09-29 16:48
Tags: 
____

#### Handler
__This is one the most important things to know


[handler](https://pkg.go.dev/net/http#Handler)

A Handler responds to an HTTP request

* `ServeHTTP` should write reply headers and data to the `ResponseWriter`
and then return.
* Returning signals that the request is finished.
* It is not valid to use the `ResponseWriter` or read from Request.Body after or concurrently with the completion of the `ServeHTTP` call
``` go
type Hanlder interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

__Also See [[GO HTTP RESPONSE]], [[GO HTTP REQUEST]]

```ad-danger

Except for ready the body, handlers should not modify the provided request
```

```ad-danger
If ServeHTTP panics, the server(the caller of ServeHTTP) assumes that the effect of the panic was isolated to the active request.

It recovers the panic, logs a stack track trace to the server error log.

```

> To write a HTTP server, we should make a `TCP` server first and the we build a HTTP server on top of that.
RFC 7230


#### `Handler` function from `net/http`

this function calls `DefaultServeMux` receiver to register handler


```go
// Handle registers the handler for the given pattern
// in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.
func Handle(pattern string, handler Handler) { DefaultServeMux.Handle(pattern, handler) }

```



##### Custom Handler
```go
package main

  

import (
	"fmt"
	"log"
	"net/http"
)


type hotdog int

func (h hotdog) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Hey, I am a HOT dog. :)))")

}

func main() {
	fmt.Println("SHIT")
	var h hotdog
	log.Fatal(http.ListenAndServe(":8080", h))

}

```



_____
##### References
1.

