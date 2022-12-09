Created: 2022-12-07 12:45
Tags: 
____

# duck typing 

```ad-quote
If it quacks, moves and swims like a duck then it should be a duck.
```

If it has a set of methods that match an interface, then you can use it wherever that interface is needed without explicitly defining that you types implement that interface.


Languages typically fall into one of two comps 
* prepare tables for all the method calls statically ( as c++ or java)
* do method lookup at each call( java script and python included) and add fancy caching to make that call efficient.

GO sits halfway between the two, it has method table but compute them at run time.

[[ make sure a type implement an interface ]]

Example 
```go 
type Binary int

func (b Binary) String() string {
	return strconv.Itoa(int(b))
}

type Stringer interface {
	String() string
}

func main() {
	var b Stringer
	fmt.Printf("%T %v\n", b, b) // <nil> <nil>
	a := Binary(10)
	b = a
	fmt.Printf("%T %v\n", b, b) // main.Binary 10
}

```

![[gointer1.png]]


Interface values are represented as a two-word pair 
*  giving a pointer to information about the type stored in the interface and 
* a pointer to the associated data

> these pointer are implicit, not directly exposed to GO


![[gointer2.png]]

##### The first word 
* The first word in the interface value points at what I call an interface table or itable.
* Itable begins with some metadata about the types involved and then becomes a list of function pointers.
	-> Interface type, not the dynamic type.


#### The Second word 
* The second word in the interface value points at the actual data, in this case a copy of b. 
* The assignment var s Stringer = b make a copy of b rather than point at b.
	* if b later changes s won't
* Values stored in interface might be arbitrarily large, but only one word is dedicated ti holding the value in the interface  structure, -> so the assignment allocates a chink of memory on the heap and records the pointer in the one-word slog ( there's an obvious optimization when the value does fit in the slot)

```ad-note 
title: type switch
The Go compiler generates code equivalent to the C expression s.tab->Type to Obtain the type pointer and check it agints the desire type.

It the types match the value can be copied by dereferencing s.data
```

```ad-note 
title: function call

To call s.String(), the Go compiler generate code that does equivalent of the C expression s.tab->fun[0](s.data)

it calls the appropriate function pointer from the itable, passing the interface value's data word as the function's first
```


#### Memory optimizations

* If the interface type involved is empty, it has no methods itable serves no purpose to except to hold the pointer to the original type.
* In this case, the itable can be dropped and the value can point at the type directly

![[gointer3.png]]


```go
package main

import (
	"fmt"
	"unsafe"
)

type I interface {
	Do()
}

type A int

func (a A) Do() {}

func printPointerIntVal(p uintptr) {
	pp := *(*uintptr)(unsafe.Pointer(p))
	fmt.Println(*(*int)(unsafe.Pointer(pp)))
}

func main() {
	var s I
	s = A(1997)

	fmt.Printf("size: %d\n", unsafe.Sizeof(s)) // 16
	l := uintptr(unsafe.Pointer(&s))
	l += 8
	printPointerIntVal(l) // 1997
}
```

#### Effective GO

1. Interfaces in Go provide a way to specify the behavior of an object
	1. if something can do this, then it can be used here
2. Interface with only one or two methods are common in Go code.
3. They are usually given a name derived from the method such as io.Writer for something that implements Write


#### Exporting an interface

If a type exist only to implement an interface and will never have exported methods beyond that interface, there is no need to export the type itself

Exporting just the interface makes it clear the value has no interesting behavior beyond  what is described in the interface

It also avoids the need to repeat the documentation on every instance of a common methods 

In such cases, the constructor should return an interface value rather than the implementing type.


#### Interface checks

1. At compile time:
	1. A type need not declare explicitly that it implements an interface.
	2. in practice, most interface conversions are static and therefore checked at compile time.
	3. passing an `*os.File` to a function expecting an `io.Reader` will not compile unless `*os.File` implements the interface.
2. At runtime:
	1. When the JSON encode receives a value that implements that interface, the encoder invokes the value's marashaling method to convert it to JSON insread of doing the standard conversion.

check this for more information [[GO JSON]]

```go
	m, ok := val.(json.Marshaler)
	
	if _, ok := val.(json.Marshaler); ok {
		// do custom conversion
	}else {
		// do default conversion
	}
```
3. Sometimes it is necessary to guarantee  within the package implementing the type that it actually satisfies the interface
	1. `json.RawMessage` needs a custom JSON representation, it should implement `json.MArshaler`, but there are no static conversions that would cause the compiler to verify this automatically.

```go
	var _ json.Marshaler = (*RawMessage)(nil)
	
```

[uber-go-guide-verify-interface-compliance](https://github.com/uber-go/guide/blob/master/style.md#verify-interface-compliance)

#### GO Comments

[link](https://github.com/golang/go/wiki/CodeReviewComments#interfaces)

1. Go interface generally belong in the package that uses the values of the interface type, not the package that implements those values.
2. The implementing package should return concrete (usually pointer or struct) -> new methods can be added to implementations without requiring extensive refactoring.
3. Do not define interfaces on the implementor side of an API "for mocking", instead, design the API so that it can be tested using the public API of the real implementation.
4. Do not define interfaces before they are used.
	1. without a realistic example of usage, it is too difficult to see whether an interface is even necessary, let alone what methods it ought to contain

Exporting an interface

Writing good interfaces is difficult, it's easy to pollute the "API" of a package by exposing bread or unnecessary interface
_____
##### References
1. https://research.swtch.com/interfaces


