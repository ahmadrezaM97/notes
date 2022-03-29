# Go Server Handler
Created: 2022-03-31 03:23
Tags: #go #http-server #http
____

```go
  package main
   
   import (
   	"log"
   	"net/http"
   )
   
   func main() {
   	log.Fatal(http.ListenAndServe(":8080", nil))
   }
```

What is the nil second argument above?
- should be handler 
- if it is specified as nil, it defaults to [[DefaultServeMux]]


_____
##### Refrences
1.

