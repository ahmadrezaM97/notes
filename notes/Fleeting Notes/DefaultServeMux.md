# DefaultServeMux
Created: 2022-03-31 03:27
Tags: 
____

#### What is DefaultServerMux?

ServeMux is an HTTP request multiplexer.
It matches the URL of each incoming request against a list of registered
patterns and calls the handler for the pattern that
most closely matches the URL.

```go
type ServeMux struct {
	mu sync.RWMutex
	m map[string]muxEntry
	es []muxEntry // slice of entries sorted from longest to shortest.
	hosts bool // whether any patterns contain hostnames
}

type muxEntry struct {
	h Handler
	pattern string

}

// NewServeMux allocates and returns a new ServeMux.

func NewServeMux() *ServeMux { return new(ServeMux) }

// DefaultServeMux is the default ServeMux used by Serve.

var DefaultServeMux = &defaultServeMux

var defaultServeMux ServeMux

```

```go
func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request) {
	handler := sh.srv.Handler
	if handler == nil {
		handler = DefaultServeMux
	}
	if req.RequestURI == "*" && req.Method == "OPTIONS" {	
		handler = globalOptionsHandler{}
	}	
	handler.ServeHTTP(rw, req)
}
```

_____
##### Refrences
1.

