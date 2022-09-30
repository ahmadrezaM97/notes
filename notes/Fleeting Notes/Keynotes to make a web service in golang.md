# Keynotes to make a web service in golang
Created: 2022-10-08 01:11
Tags: 
____

1. using a private function like `run` is a good idea

```go
package main

import (
	"log"
	"os"
)

func main() {
	if err := run(); err != nil {
		log.Println("error", err)
		os.Exit(1)
	}
}
func run() error {
	// some programming here:D 
}
```

2. logging is great 

```go
//*log.Logger

logger := log.New(os.Stdout, "app :", log.LstdFlags|log.Lmicroseconds)


```

-> log for debugging

everything from startup is better than singleton (think about that)

logging configs in startup is a good idea
	do not print secrets please :D

print version in start


3. configuration 

``` bash
go build -mod=readonly -ldflags "-X main.build=${VCS_REF}"
```

 
-> command line

4. debug server

initilaze debug server enable disable in config

5. pprofing in default servermux
6. shutdown 


```go
	srv := http.Server{
		ReadTimeout:  time.Minute,
		WriteTimeout: time.Minute,
	}
```



_____
##### References
1 .https://www.youtube.com/watch?v=IV0wrVb31Pg

