# GO HTTP LISTEN AND SERVE
Created: 2022-09-29 16:48
Tags: 
____


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



_____
##### References
1.

