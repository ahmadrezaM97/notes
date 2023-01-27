# Generic
Created: 2023-01-27 19:33
Tags: 
____

New language features in Go 1.18

1. type parameters for functions and types
2. type sets defined by interfaces
3. type interfaces

```ad-note 
title: type parameter
A type parameter gives you the ability, to parameterize a function or atype with types


`[P, Q constraint1, R contraint2]`
```


```ad-tip
title: type parameter list
Type parameter lists look like ordinary parameter lists witch square bracketes. `[]`
It is customary to start type parameters which upper-case letters to emphasize that they are types.


```

```go
func min(x, y float64) float64 {
	if x < y {
		return x
	}

	return y
}

type min[T contraints.Ordered](x,y T) T {
	if x < y {
		return x
	}

	return y
}

intMin := min[int](2,3)
floatMin := min[float64](2.0, 3.0)
```


#### instantiation

1. substitute type arguments for type parameters.
2. check that type arguments implement their constraints.
__Instantiation fails if step 2 fails.__


```ad-warning 


```

```go

type Tree[T any] struct{
	left, right *Tree[T]
	data  T
}

func (t *Tree[T]) Lookup(x T) *Tree[T]

var stringTree Tree[String]
```





A generic type example


A type parameter list may contain one and more type parameter declarations which are enclosed in square brackets and separated by commas.





```go
package cache

import "sync"

/*
	Cache is a generic type, comparing to non-generic types, there is an extra part, a type parameter list, in the declation(sepecification ,more precisely speaking) of a generic type.

The type parameter list of the `Cache` generic type is [T comparable, V any]
	

*/
type Cache[T comparable, V any] struct {
	data map[T]V
	lock sync.RWMutex
}

func New[T comparable, V any]() *Cache[T, V] {
	return &Cache[T, V]{
		data: make(map[T]V),
	}
}

func (c *Cache[T, V]) Get(key T) (V, bool) {
	c.lock.RLock()
	defer c.lock.RUnlock()

	value, ok := c.data[key]

	return value, ok
}

func (c *Cache[T, V]) Set(key T, value V) {
	c.lock.Lock()
	defer c.lock.Unlock()

	c.data[key] = value
}
```


```go
package main

import (
	"fmt"

	"github.com/ahmadrezam97/generic/cache"
)

func main() {

	c := cache.New[int, string]()

	c.Set(1, "Ahmadreza")
	c.Set(2, "Marashi")

	fmt.Println(c.Get(1))
	fmt.Println(c.Get(2))
	// Ahmadreza true
	// Marashi true
}
```


#### A generic function example


```go
package main

import "fmt"

func NoDiff[V comparable](vs ...V) bool {
	for i := range vs {
		if vs[i] != vs[0] {
			return false
		}
	}

	return true
}

func main() {

	fmt.Println(
		NoDiff(1, 1, 1, 1),
		NoDiff[int](1, 2, 3),
	)
}
```



Since Go.18, value types falls into two categories
1. Type parameter type.
2. Ordinary types.


_____
##### References
1.

