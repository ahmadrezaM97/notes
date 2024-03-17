# Golang Question
Created: 2022-09-27 05:18
Tags: 
____
### TODO:  should refactor this page to different notes


###  [[Why pointer to a slice is useful?]]

[[golang type system]]

the question is "Aren't slices already pointers to the underlying data?"

I know we can pass a slice to a function and modify it

```go
func main() {  
    slice:= []string{"a","a"}   
    func(slice []string){  
        slice[0]="b";  
        slice[1]="b";  
        fmt.Print(slice)  
    }(slice)  
    fmt.Print(slice)   // {b,b}
}
```

> using pointers leads to the same result

So Why? :D
![[go-slice.jpg]]


```ad-tip
title: When?
The pointer to a slice is indispensable when the function is going to modify the structure, the __size__ or the __location in memory__ of the slice and very change should to be visible to those who call the function.
```

when we pass a slice to a function as an argument the values of the slice are passed by reference([every_things_in_go_is_pass_by_reference]).
__BUT all the metadata describing the slice itself are just copy.__


We can modify the data of the slice in the function, however if the pointer to the data changes for any reason  or the slice modified is modified the change can be partially or no visible at all to the outside of function
``` go
func main() {  
slice:= []string{"a","a"}  
  
func(slice []string){  
	slice= append(slice, "a")  
	slice[0]="b";  
	slice[1]="b";  
	fmt.Print(slice)  
	}(slice)  
fmt.Print(slice)  
}
```

### [[Copying a sync type]]
for all these types `mutex`, `waitgroup` and .. these is a hard rule to follow
__They should never be copied__

* [[sync.Cond]]
* [[sync.Map]]
* [[sync.Mutex]]
* [[sync.RWMutex]]
* [[sync.Once]]
* [[sync.Pool]]
* [[sync.WaitGroup]]

```ad-danger
title: We may face the issue in the following conditions:

1- calling a method with a [[value receiver]]
2- calling a method with a `sync` argument
3- calling a fuction with an argumetn that contains a `sync` field
```
example:
``` go

type Counter struct {
	mu       sync.Mutex
	counters map[string]int
}

func NewCounter() Counter {
	return Counter{counters: make(map[string]int)}
}

func (c Counter) Inc(name string) {
	c.mu.Lock()
	defer c.mu.Unlock()
	c.counters[name]++
}
// soloution : use pointeror pointer reciver

```

```go
package main

import (
	"sync"
	"time"
)

func main() {
	wg := sync.WaitGroup{}

	wg.Add(1)
	go func(wg sync.WaitGroup) {
		time.Sleep(1 * time.Second)
	}(wg)
	wg.Wait()

}
// deadlock : all goroutines are asleep

// soloution: use pointer or clouser

```


We can also use `go vet`  command to check if there is any copied `sync`  value.

As a rule of thumb:
	whenever multiple goroutines have to access a common `sync` element, we must ensure that they all rely the same instance.
	


### Why is my nil error value not equal to nil?
Under the covers, interfaces are implemented as two elements, a type `T` and a value `V`.
`V` is a concrete value such as an `int`, `struct` or pointer, never an interface itself, and has type `T` .
If we store the `int` value 3 in an interface, the resulting interface value has, schematically (`T=int, V=3`).
The value `V` is also known as the interface's dynamic value, since a given interface variable might hold different values `V` during the execution of the program.

__An interface value is `nil` only if the `V` and `T` are both unset.
(`T=nil, V is not set`), in particular, a `nil` interface will always hold a `nil` type. if we store a `nil` pointer of type `*int` inside an interface value, the inner type will be `*int` regardless of the value of the pointer(`T==*int,V=nil`)
such an interface value will therefore be non-nil even when th pointer value `V` inside is `nil`



### What interface values are really?
![[interface-value.jpeg]]


Go's interface values are really a pair of pointers, when you put a concrete value into an interface value, one pointer starts pointing at the value.
The second will now point to the implementation of the interface for the type of the concerete value.

### [[ interface pollution ]]

What makes Go interfaces so different is that they are satisfied implicitly.

The bigger the interface, the weaker the abstraction
-Rob Pike

When to use interfaces ( just a general idea)

* Common behavior
* Decoupling
	* liskov substitution
	* unittest
* Restricting behavior


Abstraction should be discovered, not created

Don't design with interfaces, discover them
-Rob Pike


```ad-note
title: which side?

Go interfaces generally belong in the package that use values of the interface type.

not the package that implements those values.

The implementing package shoudl return concrete (usually pointer or struct) types.

-> New methods can be added to implemntations without requiring extensive refactoring.

DO NOT define interfaces on the implementor side of an API "for mocking"; instead, design the API so that it can be tested using the public API for the real implementation.

Do not define interface before they used:
	Without a realistic example of usage, it is too difficult to see whether an interface is even necessary.


```

[[Interface Segregation Principle]]
__Clients should not be forced to depend on interfaces that they do not use__




[go wiki code review][https://github.com/golang/go/wiki/CodeReviewComments#interfaces]


example:
[io.copy](https://github.com/golang/go/blob/c170b14c2c1cfb2fd853a37add92a82fd6eb4318/src/io/io.go#L363)

[color interface ](https://github.com/golang/go/blob/2d6f8cc2cdd5993eb8dc80655735a38ef067af6e/src/image/color/color.go#L10-L19)


```ad-note
title: BUT

If a type exists only to implement an interface and will never have exported methods beyound that interace, the se no need to export the type itself.
```
what's the benefit of returning an interface over a concrete type anyway?

returning an interface allows you to have functions that can return multiple concrete types.
for example

__Accept interfaces, return structs___
interfaces provides abstraction to the actual implementations.


__Finally, Private interfaces don't have to deal with these considerations a they're not exposed
* we can have larger interface such as [gobType](https://github.com/golang/go/blob/c170b14c2c1cfb2fd853a37add92a82fd6eb4318/src/encoding/gob/type.go#L167-L173) from `encoding/gob` package
* interfaces can be duplicated across packages, such as the timeout interface in [os](https://github.com/golang/go/blob/c170b14c2c1cfb2fd853a37add92a82fd6eb4318/src/os/error.go#L35-L37) and [net](https://github.com/golang/go/blob/c170b14c2c1cfb2fd853a37add92a82fd6eb4318/src/net/net.go#L494-L496)


### [[Interface]]


### Why pointer to a channel


### Why pointer to a map

### Why pointer to a struct


### What is nil chan and when is useful?

### nil interface

### interface best practice

### golang memory management

### golang type system

### Struct golang


### Memory leak with slice

### Memory leak with map

### know how iterate over slice works

### What and When generic

### What and when reflection

### bufio?

#### efficient string building

### benchmark

### profiliing



1.

