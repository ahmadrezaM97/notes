# Generic
Created: 2023-01-27 19:33
Tags: 
____

New language features in Go 1.18

1. type parameters for functions and types
2. type sets defined by interfaces
3. type inference

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
Types can have type parameter lists, too.


Methods declare the respective type parameters witch the receiver.
```

```go

type Tree[T any] struct{
	left, right *Tree[T]
	data  T
}

func (t *Tree[T]) Lookup(x T) *Tree[T]

// istantiations
var stringTree Tree[String]
```


### Type sets

kind of types that a type parameter can be instantiated with

1. Ordinary parameter list have a type for each value parameter.
	1. This type defines a set of values
2. Type parameter lists also have a type for each type parameter
	1. This type defines a set of types. 
		1. __IT IS CALLED THE TYPE CONSTRAINT__


```go
type min[T contraints.Ordered](x,y T) T {
	if x < y {
		return x
	}

	return y
}

/*

`constrints.Ordered` has two functions:
1. only types witch orderable values can be passed as type arguments to T.
2. Values of type `T` can e used as operands for `<` in the function body.
*/
```

```ad-danger 
Type Constraints are interfaces.
```

__Until recently interface defines a set of methods
The different views__

![[generic-interface.png]]

The different view 
	__Interface defines a set of type

![[generic-interface-2.png]]

__We can add type explicitly to an interface__

![[interface-generic-2.png]]

```go
package constraints

type Ordered interface {
	int | float | ~string
}
```

```ad-tip
~ is a new token added to Go.
~T means the set of all types with underlyning type T.
```


You can define an interface as type constraint
```go

type SHIT int

func (s SHIT) Do() string {
	return "SHIT"
}

type MyInterface interface {
	~int | float64
	Do() string
}

func f[V MyInterface](v V) {
	fmt.Println(v.Do())
}

func main() {
	f(SHIT(2))
}
```


### The two functions of a type constraint

1. The __type set__ of __a constraint__ is  __a set of valid type arguments__.
2. If __all types__ in the constraint __support an operation__, that operation may be used with respective type parameter. ( * some restriction apply ( I don't know what))


#### Constraint literals

```go 
/*
[S inteface{~[]E}, E interface{}]
[S ~[]E, E any]
[S ~[]E, E any]
*/

func elementExist[S ~[]E, E comparable](arr S, element E) bool {
	for i := range arr {
		if arr[i] == element {
			return true
		}
	}

	return false
}

func main() {
	fmt.Println(elementExist([]int{1, 2, 3, 4, 5, 6}, 10))
	fmt.Println(elementExist([]string{"a", "b", "c"}, "b"))

}


```



### type inference
```go

func min[T constraints.Ordered](x,y T) T

m = min(1,2)

```
__ passing type arguments  leads to more verbose code__




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

