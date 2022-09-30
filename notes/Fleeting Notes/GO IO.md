


# GO IO
Created: 2022-09-29 21:55
Tags: 
____

#### ioutils

we can read all data from `io.Reader` with `ioutil.Reaeder` or `io.Reader`
```go
	request := struct {
		Name string `json:"name"`
	}{}
	data, err := ioutil.ReadAll(r.Body)
	json.Unmarshal(data, &request)
```

https://www.gobeyond.dev/io/


_____
##### References
1.

https://tutorialedge.net/golang/reading-console-input-golang/