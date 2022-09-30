# GO HTTP RESPONSE
Created: 2022-09-29 16:50
Tags: 
____

#### Introduction to  HTTP `ResponseWriter`

[http.ResponseWriter](https://godoc.org/net/http#ResponseWriter)

* A `ResponseWriter` interface used by an HTTP handler to construct a HTTP response
* this interface has ` Write([]byte) (int, error)` function that means it would satisfied `Writer` interface


```ad-danger
A `ResponseWriter` may not be used after the `Handler.ServeHTTP` method has retured
```

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

* A response represents the server side of an HTTP response.
* `response`  is a private struct in `net/http/server.go` that implement `ResponseWriter` interface

#### Header

* A Header represents the key-value pairs in an HTTP header.
* The keys should be in canonical form, as returned by CanonicalHeaderKey

``` go
type header map[string][]string
```

An `http.ResponseWriter` has a method `Header()` witch returns a  `http.Header`.
Look at the documentation for `http.Header`.


##### methods

```go 
func (h Header) Set(key, value string)
```
* Set sets the header entries associated with key to the single element value
* it replaces any existing values associated with key.

example:
```go
w.Header().Set("Allow", "POST")
```

 the key is case insensitive

``` go
type header map[string][]string


// Set a new cache-control header. If an existing "Cache-Control" header exists
// it will be overwritten.
w.Header().Set("Cache-Control", "public, max-age=31536000")

// In contrast, the Add() method appends a new "Cache-Control" header and can
// be called multiple times.
w.Header().Add("Cache-Control", "public")
w.Header().Add("Cache-Control", "max-age=31536000")

// Delete all values for the "Cache-Control" header.
w.Header().Del("Cache-Control")

// Retrieve the first value for the "Cache-Control" header.
w.Header().Get("Cache-Control")

// Retrieve a slice of all values for the "Cache-Control" header.
w.Header().Values("Cache-Control")

```


##### Header canonicalization

When you're using the `Set()`, `Add()`, `Del()`,`Get()` and `Values()` methods on the header map, the header name will always be canonicalized using the `textproto.CanonicalMIMEHeaderKey() `
if you need to avoid this you have use direct access

```go
w.Header()["X-XSS-Protection"] = []string{"1; mode-block"}
```






_____
##### References
1.



#### Write

```go
// Write writes a header in wire format.
func (h Header) Write(w io.Writer) error {
	return h.write(w, nil)
}

```

example of use cases
```go

w.Write([]byte("foobar"))

// or

fmt.Fprintln(w, "salam")

// and for json

resp := map[string]string{
	"message":"ahsantom!"
}

jsonResp, err := json.Marshal(resp)
if err != nil{
	log.Fatal(err)
}

w.write(jsonResp)

// or 

resp := map[string]string{"key": "value"}
err := json.NewEncoder(w).Encode(resp)
if err != nil {
		log.Fatalln(err)
}
```

also see [[GO JSON]], [[GO JSON marshall vs encode]]


#### WriteHeader

```go
w.WriteHeader(http.StatusBadRequest)
w.WriteHeader(http.StatusAlreadyReported)
```

