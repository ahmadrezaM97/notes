# GO HTTP SERVEMUX
Created: 2022-09-29 16:51
Tags: #golang #mux 
____

#### Introduction

A `servemux`(router) stores a mapping between the predefined URL paths for you application and the corresponding handlers.

```ad-tip
title: what does the word exactly mean?
In electronics, a multiplexer(or `mux`) is a device that selects one of several input signals and forwards the selected input into single line.
```

#### `DefaultServeMux`(`*ServeMux`)

`DefaultServeMux` is the default `ServeMux` used by Serve

let's look at code
```go
// DefaultServeMux is the default ServeMux used by Serve.

type ServeMux struct {
	mu    sync.RWMutex
	m     map[string]muxEntry
	es    []muxEntry // slice of entries sorted from longest to shortest.
	hosts bool       // whether any patterns contain hostnames
}

type muxEntry struct {
	h       Handler
	pattern string
}

var DefaultServeMux = &defaultServeMux
var defaultServeMux ServeMux

```


```ad-note
title: what this tells us

Any value of type `*http.ServeMux` implements the `http.Handler` interface.

__What this tells us?__

We can pass a value of type `*http.ServeMux` into `http.ListenAndServe`

```

So `DefaultServeMux` is a pointer to `ServeMux` struct, know look at document

*  `ServeMux` is an HTTP request multiplexer.
*  It matches the URL of each incoming request against a list of registered patterns and calls the handler for the pattern that most closely matches the URL.


As we said `DefaultServeMux`(`ServerMux`) implements `Handler` interface:

```go

// ServeHTTP dispatches the request to the handler whose
// pattern most closely matches the request URL.
func (mux *ServeMux) ServeHTTP(w ResponseWriter, r *Request) {
	if r.RequestURI == "*" {
		if r.ProtoAtLeast(1, 1) {
			w.Header().Set("Connection", "close")
		}
		w.WriteHeader(StatusBadRequest)
		return
	}
	h, _ := mux.Handler(r)
	h.ServeHTTP(w, r)
}

```

##### DefaultServeMux

`ListenAndServe` starts an HTTP server with a given address and handler.
The Handler is usually `nil`, which means to use `DefaultServeMux`.
__`Handle` and `HandleFunc` add handler to `DefaultServeMux`__

``` go
http.ListenAndServe(":8080", nil)
```


#### What can we do with ServeMux
1. __we can initial a `serverMux`
```go
func NewServeMux() *ServeMux { return new(ServeMux) }
```

2. __we can register an handler for the given pattern

>by handler I mean a struct that implement Handler interface

example:
```go 
type customHandler struct{}

func (c customHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "salam")
}

func main(){
	var c customHandler
	mux.Handle("/custom", c)
}

```

implementation :
```go
// Handle registers the handler for the given pattern.
// If a handler already exists for pattern, Handle panics.
func (mux *ServeMux) Handle(pattern string, handler Handler) {
	mux.mu.Lock()
	defer mux.mu.Unlock()

	if pattern == "" {
		panic("http: invalid pattern")
	}
	if handler == nil {
		panic("http: nil handler")
	}
	if _, exist := mux.m[pattern]; exist {
		panic("http: multiple registrations for " + pattern)
	}

	if mux.m == nil {
		mux.m = make(map[string]muxEntry)
	}
	e := muxEntry{h: handler, pattern: pattern}
	mux.m[pattern] = e
	if pattern[len(pattern)-1] == '/' {
		mux.es = appendSorted(mux.es, e)
	}

	if pattern[0] != '/' {
		mux.hosts = true
	}
```

3. __we can register a `handler function` for the given pattern
```go
// HandleFunc registers the handler function for the given pattern.

func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	if handler == nil {
		panic("http: nil handler")
	}
	mux.Handle(pattern, HandlerFunc(handler))
}
```

Also see [[GO HTTP Handler Func]]

```ad-important
title: net/http Handle() and HandleFunc

when we call Handle or HandleFunc functions from net/http, actually we are calling `Handler` reciver on `Servemux`

```go 
// Handle registers the handler for the given pattern
// in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.
func Handle(pattern string, handler Handler) { DefaultServeMux.Handle(pattern, handler) }


// HandleFunc registers the handler function for the given pattern
// in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}

```

4. we can listen and serve to `servemux`

```go
http.ListenAndServe(":3000", mux)
```

```ad-note
title: nil and defaultServeMux

if we pass nil as the second arg in http.ListenAndServe
we will use the `DefaultServeMux`(*ServeMux)

```go
// ListenAndServe listens on the TCP network address addr and then calls
// Serve with handler to handle requests on incoming connections.
// Accepted connections are configured to enable TCP keep-alives.
//
// The handler is typically nil, in which case the DefaultServeMux is used.
//
// ListenAndServe always returns a non-nil error.
func ListenAndServe(addr string, handler Handler) error {
	server := &Server{Addr: addr, Handler: handler}
	return server.ListenAndServe()
}
```


```ad-note
title: why we  call listenAndServe with a servemux?

the answer is ListenAndServe accept a Handler interface 

```go

func ListenAndServe(addr string, handler Handler) error {
	server := &Server{Addr: addr, Handler: handler}
	return server.ListenAndServe()

}

type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```


#### How can we build custom `mux`

```go
package main

import (
	"fmt"

	"log"

	"net/http"
)

type hotdog int

func (h hotdog) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Hey, do you want to meet a HOT-DOG??")
	switch r.URL.Path {
	case "/dog":
		fmt.Fprintln(w, "Hey, I am the dog one")
	case "/hot":
		fmt.Fprintln(w, "Hey, I am the HOT one")
	default:
		fmt.Fprintln(w, "Who the fuck are you talking to?")
	}

}

func main() {
	var h hotdog
	log.Fatal(http.ListenAndServe(":8080", h))
}

```



#### Summary

* `DefaultServeMux` is the just a pointer to `ServeMux`
	* `ServeMux` is a struct
	* `SrveMux` implements `Handler` interface => we can pass it to `ListenAndServe`
* `NewServeMux`
	* We can create a `ServeMux` by using `NewServeMux`.
* passing `nil` to `ListenAnServe` function
	* We can use the default `ServeMux` by passing `nil` to `ListenAndServe`.
* `Handler` in `net/http` just call `Handler` receiver of `ServeMux`
	* for registering  a `Handler`(implementation of `Handler` interface)
	* IT takes a value of type Handler
* `HandlerFunc` in `net/http` just call `HandlerFunc` receiver of `ServeMux`
	* for registering a `HandlerFunc`
	* `type HandlerFunc func(ResponseWriter, *Request)`
	* takes a `func` with this signature `func(ResponseWriter, *Response)`



____
#### References
1.

