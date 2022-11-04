# GO method set
Created: 2022-11-04 02:44
Tags: 
____
let start with this question "what is the difference between a pointer receiver and a value receiver? Do we can call a pointer receiver on value? 

```go
type Data struct {
	cnt int
}

func (d *Data)DoPointer(){ // value receiver
	d.cnt++
}
func (d Data)DoValue(){ // pointer receiver
	d.cnt++
}

d := Data{}

d.DoPointer() // ok
d.DoValue()// also ok

(&d).DoPointer() // ok
(&d).DoValue() // ok
```

```ad-warning

Even in cases where the compiler could take the address of a value to pass to the method, if the method modifies the value -> the changes will be lost in the caller

```

```ad-important
title: how it is possible

in some cases the compiler can take the address of a value

A method call x.m() is valid if the method set of ( the type of )x contains m and the argument list can be assigned to the parameter list of m.

if x addressable and &x's method set contains m, x.m() is shorthand for (&x).m()
```

In other words, you can call value receiver on both value and pointer, but a pointer receiver  can be called on a pointer or and addressable value
So this question come to mind what's does "addressable" mean? 
 * when the compiler can get address of a pointer

well, when compiler can not?

1 -> literal value
2 -> map key
3 -> function return 
4-> interface *

example:
```go
type Data struct{}
func (d *Data) Do() {}

func f() Data {
	return Data{}
}

func Run(){
	// function returen
	f().Do() // error

	// literal value
	Data{}.Do() // error

	// interface
	var _ Doer = Data{} // error | right on is var _ Doer = &Data{}
	
	// map example
	m := make(map[int]Data)
	m[0] = Data{}
	m[0].Do() // error
}
```



#### lets talk about method sets

* __A method set of a type `T` consists of all methods with receiver type `T`.
* __while that of the corresponding pointer type `*T` consists of all methods with receiver `*T` or `T`

```ad-summary
method set of `T` is subset of `*T`
```

let see what does it mean?

imagine you a Interface `Doer` with a `Do()` function, and type `Data` has a pointer receiver `Do` 

value Data does not satisfy `Doer` interface.
because `Data` doesn't have Do method it belongs to  pointer of `Data`
so you can not have `var _ Doer = Data{}` you should use `var _ Doer = &Data{}`
```go

type Data struct{}
func (d *Data) Do() {}

type Doer interface {
	Do()
}

// Do is pointer receiver so just *Data has it => data do NOT satisfy Doer interface
var _ Doer = Data{} // error
/*
cannot use Data{} (value of type Data) as type Doer in variable declaration:
        Data does not implement Doer (Do method has pointer receiver)
*/


var _ Doer = &Data{} // correct
```

```ad-warning
you even can not have a addresable form of `Data` that satisfy `Doer`
```go

d := Data{}
var _ Doer = d // error

```


```ad-important
title: why?

This distinction arises because 
* if an interface value contains a pointer `*T`
	* a method call can obtain a value by dereferencing the pointer
* BUT
	* if an interface value contains a value `T`
		* These is no safe way for a method call to obtain a pointer



Doing so would allow a method to modify the contens of the value inside the interface, which is not permitted by the language specification
```


```ad-note
title: What? wait? valued in an interface is what?

The concrete value stored in an interface is not addressable, in same way, that a map element is not addressable.


A non-pointer value stored in an interface isn't addressable to maintain type integrity.
```

```ad-important
title: why? why? why?

for example, a pointer to A, which point to a value of type A in an interface, would be invlidated when a value of a diffrent type B is subsequently stored in the interface.
```go
type I interface{} // or any :D
var A int
var B string

func main(){
	var a A = 5
	var i I = a
	fmt.Printf("%T\n", i)

	var aPtr *A = &(i.(A)) // not allowd, but if it were
	var b B = "hey"
	i = b
	
	fmt.Printf("i of type %T, aPtr is oftype %T\n",i, aptr)	
	
}

After putting B into i, what aPtr pointing to? it was declared as pointer to a A but now I contain a B and aPtr is no longer a valid pointer to A.

```
https://go.dev/play/p/eeaYzqZsmOs



```ad-summary
title: what did we learn?

1. if you have a `*T` you can call methods that have receiver type of `*T` and `T`
2. if you have a `T` and it is addressable you can call methods that have a receiver type of `*T` as well as `T`(t.Meth() will be equivalent to (&t).Meth())
3. if you have a `T` and it is not addressable,you can NOT get a pointer to it, so you can only call methods that have a receiver type of `T`

4. if you have an interface `I`, and some or all of the methods is `I`'S method set are provided by methods with a receiver of `*T` => `*T` satisfy `I` bot `T` does not.

```



### Should I define methods on values or pointers?

[go FAQ](https://go.dev/doc/faq#methods_on_values_or_pointers)

1. First, and most important, does the method need to __modify__ the receiver? If it does, the receiver `must` be a __pointer__
	1. __Slices__ and maps __acts__ as __references__, so their story is a little more subtle
	2. change __the length of a slice__  in a method the receiver must still be a  __pointer__
2. Second is the __consideration of efficiency__, If the receiver __is large, a big `struct` for instance__ , it will be much cheaper to use a __pointer__ receiver
3. Next is __consistency__, if some of the methods of the type must have pointer receiver, the rest should too.
4. For types such as __basic types, slices, and small `struct`, a value receiver__ is very cheap so unless the semantics of the method requires a pointer, a value receiver is efficient and clear.

[Go review comments](https://github.com/golang/go/wiki/CodeReviewComments#receiver-type)


1. If the receiver type is a __`map`, `func` or `chan`__, do __NOT__ user a __pointer__ to the. if the receiver is `slice` and the method__ doesn't reslice or reallocate the slice__, do __NOT__ use a __pointer__ to it
2. If the method needs to __mutate__ the receiver, the receiver __MUST__ be __POINTER__.
3. if the receiver is a struct that contains a `sync.Mutex` (or any similar) the receiver __MUST__ be a __pointer__. 
	1. for more details check the [Go sync package]
4. if the receiver is a large `struct` or `arrar`, a pointer receiver is more efficient
	1. How large is large? Assume it's equivalent to passing all its elements as argument to the method. if the feels too large. it's also too large for the receiver
5. if change must must be visible in the original receiver -> user pointer
6. if the receiver is a struct, array or slice and any of it's elements is a pointer to something that might be mutating, prefer a pointer receiver, as it will make the intention clearer to reader
7. if the receiver is a small array or struct that is naturally a value type( like time.Time) with no mutable fields and no pointer, or us just a simple basic type such as `int`  or `string`, a value receiver is makes sense.
	1. Do not choose a value receiver type for this reason without profiling first.
8. DO not mix receiver types, choose either pointer or struct types for all available methods
9. when is doubt -> pointer
_____
##### References
1. [go FAQ](https://go.dev/doc/faq#different_method_sets)

