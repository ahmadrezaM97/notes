# Go Net-HTTP
Created: 2022-05-26 21:46
Tags: 
____

#### Handler
__This is one the most important things to know

[handler](https://pkg.go.dev/net/http#Handler)

``` go
type Hanlder interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

```ad-note

`ServerHTTP` should write reply headers and data to the ResponseWriter and the return.

Returning signals that the request is finished.

__It is not valid to use the ResponseWriter or read from REquest.Body after or concurrencylu with the completion of the `ServeHTTP call`__


```


```ad-danger
If ServeHTTP panics, the server(the caller of ServeHTTP) assumes that the effeect of the panic was isolated to the active request.

It recovers the panic, logs a stack track trace to the server error log.


```

> To write a HTTP server, we should make a TCP server first and the we build a HTTP server on top of that.
RFC 7230

##### HotDog HttpHandler
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



#### ListenAndServe

[http.ListenAndServe](https://pkg.go.dev/net/http#ListenAndServe)
[http.ListenAndServeTLS](https://godoc.org/net/http#ListenAndServeTLS)

```  go

func ListenAndServer(addr string, handler Handler) error
func ListenAndServeTLS(addr, certFile, keyFile string, handler Handler) error

```

```ad-note
title: ListenAndServe

ListenAndServe listens on the TCP network address addr and then class Serve with handler to handle requests on incoming connections.
Accepted connections are configured to enable TCP keep-alives.
The handler is typically nil, in wich case the DEfaultServeMux is used

ListenAndServe always returns a nont-nil error




```
##### Example
``` go 

import (
	"io"
	"log"
	"net/http"
)


func main() {
	
	helloHandler := func(w http.ResponseWriter, req *http.Request){
	io.WriterString(w, "Hi\n")
	
	}
	http.HandlerFunc("/hello", helloHandler)
	log.Fatel(http.ListenAndServe(":8080",nil))
	
}

```

#### Request
See [http.Request](https://godoc.org/net/http#Request) in the documentation.

Here it is with most of the comments and some of the fields stripped put:
``` go 
type Request struct {
    Method string
    URL *url.URL
	//	Header = map[string][]string{
	//		"Accept-Encoding": {"gzip, deflate"},
	//		"Accept-Language": {"en-us"},
	//		"Foo": {"Bar", "two"},
	//	}
    Header Header
    Body io.ReadCloser
    ContentLength int64
    Host string
    // This field is only available after ParseForm is called.
    // Form contains the parsed from data
    // including both the URL field's query paramaters and the POST or PUT from
    // data.
    // The HTTP client ignores From and uses Body instead.
    
    Form url.Values
    // This field is only available after ParseForm is called.
    PostForm url.Values
    MultipartForm *multipart.Form
    // RemoteAddr allows HTTP servers and other software to record
	// the network address that sent the request, usually for
	// logging. 
    RemoteAddr string
}
```



#### URL

The `http.Request` type is a struct which has a `URL` field. Notice that the type is a `*url.URL`

Take a look at type `url.URL`
``` go
type URL struct {
    Scheme     string
    Opaque     string    // encoded opaque data
    User       *Userinfo // username and password information
    Host       string    // host or host:port
    Path       string
    RawPath    string // encoded path hint (Go 1.5 and later only; see EscapedPath method)
    ForceQuery bool   // append a query ('?') even if RawQuery is empty
    RawQuery   string // encoded query values, without '?'
    Fragment   string // fragment for references, without '#'
}

```


#### Header
``` go
type header map[string][]string
```

An `hhtp.REsponseWriter` has a method `Header()` witch returns a  `http.Header`.
Look at the documentation for `http.Header`.

``` go
type Header
func (h Header) Add(key, value string)
func (h Header) Del(key string)
func (h Header) Get(key string) string
func (h Header) Set(key, value string)
func (h Header) Write(w io.Writer) error
func (h Header) WriteSubset(w io.Writer, exclude map[string]bool) error
```

We can set headers for a response like this

``` go

res.Header().Set("key","value")

```


#### Response
[http.ResponseWriter](https://godoc.org/net/http#ResponseWriter)


``` go
type ResponseWriter interface {
    // Header returns the header map that will be sent by
    // WriteHeader. Changing the header after a call to
    // WriteHeader (or Write) has no effect 
    Header() Header

    // Write writes the data to the connection as part of an HTTP reply.
    //
    // If WriteHeader has not yet been called, Write calls
    // WriteHeader(http.StatusOK) before writing the data. If the Header
    // does not contain a Content-Type line, Write adds a Content-Type set
    // to the result of passing the initial 512 bytes of written data to
    // DetectContentType.
    Write([]byte) (int, error)

    // WriteHeader sends an HTTP response header with status code.
    // If WriteHeader is not called explicitly, the first call to Write
    // will trigger an implicit WriteHeader(http.StatusOK).
    // Thus explicit calls to WriteHeader are mainly used to
    // send error codes.
    WriteHeader(int)
}
```
[[GO web-server]]



_____
##### References
1.




#### ServeMux
-   router
-   request router
-   multiplexer
-   mux
-   servemux
-   server
-   http router
-   http request router
-   http multiplexer
-   http mux
-   http servemux
-   http server




In electronics, a multiplexer(or mus) is a device that selects one of several input siganls and forwards the selected input into single line.

``` go
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
		fmt.Fprintln(w, "What the HELL are you talking about? the isn't anyone")
	}

}

func main() {
	var h hotdog

	log.Fatal(http.ListenAndServe(":8080", h))

}
```

```ad-danger
title: IMPORTANT

Any value of type `*http.ServeMux` implements the `http.Handler` interface.

__What this tells us?__

We can pass a value of type `*http.ServeMux` into `http.ListenAndServe`

```


```ad-note
title:ServeMux

* `ServeMux`
	* `NewServeMux`
		* We can create a `ServeMux` by using `NewServeMux`.
	* `DefaultServeMux`
		* We can use the default `ServeMux` by passing `nil` to `ListenAndServe`.
* `Handler`
	* Takes a value of type Handler
* `HandlerFunc`
	* takes a `func` with this signature `func(ResponseWriter, *Response)`

``` go
func NewServeMux() *ServeMux

// methods of ServeMux

//takes a route AND Handler
func (mux *ServeMux) Handle(pattern string, handler Handler)

// takes a route AND a function
func (mux *ServeMux) HandleFunc(patttern string, handler func(ResponseWriter, *Request))


func (mux *ServeMux) Handler(r *Request)(h Handler, pattern string)
func (mux *ServeMux) ServeHttp(w ResponseWriter, r *Request)

```

 

##### DefaultServeMux

`ListenAndServe` starts an HTTP server with a given address and handler.
The Handler is usually `nil`, which means to use `DefaultServeMux`.
__`Handle` and `HandleFunc` add handler to `DefaultServeMux`__

``` go
http.ListenAndServe(":8080", nil)
```



#### Disambiguation

``` go

// func(ResponseWriter, *Request)  vs HandlerFunc


package betterone

import (
	"fmt"
	"log"
	"net/http"
)

func CatHanlder(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "SHIT, Am I a cat here?")
}

func BenanaHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Is hot benana even a word? am I joke to you?")
}

func RunABetterOneApproach() {

	// LOOK AT DIFFRENCE
	http.HandleFunc("/cat", CatHanlder)
	// OR
	http.Handle("/benana/", http.HandlerFunc(BenanaHandler))

	log.Fatal(http.ListenAndServe(":8080", nil))
}

```

__This is How `HandleFunc` Implement

``` go
  
// HandleFunc registers the handler function for the given pattern.
func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	if handler == nil {
		panic("http: nil handler")
	}
	mux.Handle(pattern, HandlerFunc(handler))
}
```
