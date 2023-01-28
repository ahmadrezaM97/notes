# Generic
Created: 2023-01-27 19:33
Tags: 
____

New language features in Go 1.18

1. __Type parameters for functions and types
2. __Type sets defined by interfaces
3. __Type inference

### Type parameter

__Functions__ and __Types__ are now premitted to have type parameters.

A type parameter list looks like an ordinary parameter list, expect that it uses square brackets instead of parentheses.

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

// these are called instantiation
intMin := min[int](2,3)
floatMin := min[float64](2.0, 3.0)
```

##### Instantiation

1. Substitute type arguments for type parameters.
2. Check that type arguments implement their constraints.
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


* An ordinary function has a type for each value parameter; that type defines a set of values.
	* For instance, if we have a `float64` type as in the non-generic function `Min`, the premissible set of argument values ins the set of floating-point values that can be represented by the `float64` type.
* Similarly, type parameter lists have a type for each type parameter.
	* Because a type parameter is itself a type, __the types of type parameters defines sets of types__
	* This meta-type is called a __TYPE CONSTRAINT__

```ad-important
In GO, type constraints must be interfaces.
That is, an interface type can be used as a value type, and it can also be used as a meta-type.
```

* Interfaces define methods, so obviously we can express type constraints that require certain method to be present 
```go

type SomethingWired int

func (s SomethingWired) Do() string {
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
	f(SomethingWired(2))
}
```

* BUT `constraints.Ordered` is an interface type too, and the `<` operator is not a method. (`WTF`? :D)
	* To make this work, we look at interfaces in a new way.


(first view)
__Until recently interface defines a set of methods__, which is roughly the set of methods enumerated in the interface.
Any type that implements all those methods implements the interface

![[generic-interface.png]]

(second view)
But another way of looking at this is __to say that the interface defines a set of types, namely the types that implement those methods.__
From this perspective, any type that is an element of the interface's type set implements the interface.

![[generic-interface-2.png]]

The two views lead to the same outcome
__for each set of methods we can imagine the corresponding set of types that implement those methods, and that is the set of types defined by the interface.__


__We can extended the syntax for inteface types to make this work__
```go
interface { int|string|bool}
```

![[interface-generic-2.png]]

```go
package constraints

type Ordered interface {
	int | float | ~string
}
```

```ad-tip
For type constraints we usually don't care about a specific type, such as `string`; we are interested in all string types.
That is what ~ token is for.
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

