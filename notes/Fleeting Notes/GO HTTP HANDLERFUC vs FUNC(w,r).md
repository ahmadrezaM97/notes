# GO HTTP HANDLERFUC vs FUNC(w,r)
Created: 2022-09-29 16:51
Tags: 
____
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



_____
##### References
1.

