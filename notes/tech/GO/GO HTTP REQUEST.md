# GO HTTP REQUEST
Created: 2022-09-29 16:48
Tags: 
____

#### Request
See [http.Request](https://godoc.org/net/http#Request) in the documentation.

Here it is with most of the comments and some of the fields stripped put:
``` go 
type Request struct {
	
	// Method specifies the HTTP method (GET, POST, PUT, etc.).
	// for client requests, an empty string means GET.
    Method string

	// URL specifies either the URL being requested(for server requests) or th URL to access( for client request)
    
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


#### convert request body to json

```go
	request := struct {
		Name string `json:"name"`
	}{}
	err := json.NewDecoder(r.Body).Decode(&request)
	if err != nil {
		log.Fatalln(err)
	}
	log.Println("request", request)

// or 

	request := struct {
		Name string `json:"name"`
	}{}
	data, err := ioutil.ReadAll(r.Body)
	json.Unmarshal(data, &request)


```


#### read query param  
```go 

func Simple(w http.ResponseWriter, r *http.Request) {
	name := r.URL.Query().Get("name")
	response := map[string]string{
		"name": name,
	}

	err := json.NewEncoder(w).Encode(response)
	if err != nil {
		log.Println(err)
	}
}
```


### Context
* Context returns the request's context
* To change the context -> use `r.WithContext`
* the returned context is always non-nil ( default is `context.Background`)
* for outgoing client request , the context controls cancellation.
* for incoming request server, the context is canceled when client's connection closes ( with http/2 is possible ) or when `ServerHTTP` method return 

```go 

func (r *Request) Context() context.Context {
	if r.ctx != nil {
		return r.ctx
	}
	return context.Background()
}
```





_____
##### References
1.

