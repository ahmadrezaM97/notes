# signleflight
Created: 2023-07-31 03:51
Tags: 
____

```go
package main

import (
	"fmt"
	"log"
	"math/rand"
	"sync"
	"time"

	"golang.org/x/sync/singleflight"
)

func main() {
	if err := run(); err != nil {
		log.Fatalln(err)
	}
}

func run() error {
	var wg sync.WaitGroup

	var g singleflight.Group

	wg.Add(1)
	go func() {
		defer wg.Done()
		v, err, shared := g.Do("key", func() (any, error) {
			time.Sleep(5 * time.Second)
			return 5, nil
		})

		fmt.Println("Goroutine result : ", v, err, shared)
	}()

	wg.Add(1)
	go func() {
		defer wg.Done()
		v, err, shared := g.Do("key2", func() (any, error) {
			time.Sleep(5 * time.Second)
			return 12, nil
		})
		fmt.Println("Goroutine result : ", v, err, shared)
	}()

	wg.Wait()

	return nil
}
```

_____
##### References
1.

