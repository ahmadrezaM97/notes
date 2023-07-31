Created: 2022-11-26 16:46
Tags: 
____
### Overview

Golang does not support inheritance but supports association via embedding 
This can be done in three ways
1. Interfaces in interfaces
2. Interfaces in structs
3. Structs in structs

```ad-danger
we can not embed struct in interface
```

### which type can be embeded

An embedded field must be specified as a type name `T` or as a pointer to a non-interface type name `*T`, and `T` itself may not be a pointer type.

__A struct type can not be embed itself or its aliases, recursively__

#### what is the meaningfulness of type embedding?

The main purpose of type embedding is to extend the functionalities of the embedded types into embedding type.
so that we don't need to re-implement the functionalities.

* if type `T` inherits another type, the type `T` obtain the abilities of the other type, at the same time, each value of type `T` can also be viewed as a value of the other type.
* T a type `T` embeds another type, then type other type becomes a part of type `T` and type `T` obtains abilities of the other type.
	* BUT none values of type `T` can be viewed as values of the other type.


### shorthands of selectors

if middle name in a selector corresponds to an embedded filed, then that name can be omitted from selector
```go
package main

type A struct {
	FieldX int
}

func (a A) MethodA() {}

type B struct {
	*A
}

type C struct {
	B
}

func main() {
	var c = &C{B: B{A: &A{FieldX: 5}}}

	// The following 4 lines are equivalent.
	_ = c.B.A.FieldX
	_ = c.B.FieldX
	_ = c.A.FieldX // A is a promoted field of C
	_ = c.FieldX   // FieldX is a promoted field

	// The following 4 lines are equivalent.
	c.B.A.MethodA()
	c.B.MethodA()
	c.A.MethodA()
	c.MethodA() // MethodA is a promoted method of C
}
```
#### Selector shadowing and colliding

for a value `x`, it is possible that man ot its full-form selector have the same last item `y` and every middle name of the

### Embedding struct in struct

```go
package main

import "fmt"

type Base struct {
	num int
}

func (b Base) describe() string {
	return fmt.Sprintf("it's my description %d", b.num)
}

type container struct {
	Base
	name string
}

func main() {
	co := container{
		Base: Base{
			num: 1,
		},
		name: "simple container",
	}

	fmt.Println(co.name)
	fmt.Println(co.describe())
	fmt.Println(co.num)
	/*
		simple container
		it's my description 1
		1
	*/
}
```


Exported filed will promoted

you can use it in decorator pattern



_____
##### References
1.

