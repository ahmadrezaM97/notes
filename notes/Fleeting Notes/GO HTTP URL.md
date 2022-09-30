# GO HTTP URL
Created: 2022-09-29 16:49
Tags: 
____


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




_____
##### References
1.

