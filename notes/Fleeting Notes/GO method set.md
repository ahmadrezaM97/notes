# GO method set
Created: 2022-11-04 02:44
Tags: 
____
#### Why Do `T` and `*T` have different method sets?


* __A method set of a type `T` consists of all methods with receiver type `T`.
* __while that of the corresponding pointer type `*T` consists of all methods with receiver `*T` or `T`

```ad-note
method set of `T` is subset of `*T`
```

__what it does NOOOOT  mean?__
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
// `T` has just DoValue, and `*T` has both Dovalue and DoPointer
d.DoPointer() // ok
d.DoValue()// also ok

(&d).DoPointer() // ok
(&d).DoValue() // ok

```

```ad-important
title: how it is possible

in some cases the compiler can take the address of a value

A method call x.m() is valid if the method set of ( the type of )x contains m and the argument list can be assigned to the parameter list of m.

if x addressable and &x's method set contains m, x.m() is shorthand for (&x).m()
```

__Both value receiver and pointer receiver methods can be invoked on a correctly-typed pointer or non-pointer 

__Regardless of what the method is called on, within the method body the identifier of the receiver refers to a by-copy value when a value receiver is used, a pointer when a pointer receiver is used

####  So what is the difference?

```go

type Data struct{}
func (d *Data) Do() {}

type Doer interface {
	Do()
}

// Do is pointer receiver so just *Data has it => data do NOT satisfy Doer interface
var _ Doer = Data{}
/*
cannot use Data{} (value of type Data) as type Doer in variable declaration:
        Data does not implement Doer (Do method has pointer receiver)
*/

```


```ad-important
title: why?

This distinction arises because 
* if an interface value contains a pointer `*T`
	* a method call can obtain a value by dereferencing the pointer
* BUT
	* if an interface value contains a value `T`
		* These is no safe way for a method call to obtain a pointer
		* ___Doing so would allow a method to modify the contens of the value inside the interface, which is not permitted by the language specification
```

Even in cases where the compiler could take the address of a value to pass to the method, if the method modifies the value -> the changes will be lost in the caller


#### Should I define methods on values or pointers?

[go FAQ](https://go.dev/doc/faq#methods_on_values_or_pointers)
* First, and most important, does the method need to modify the receiver?
	* If it does, the receiver `must` be a pointer
		* __Slices and maps acts as references, so their story is a little more subtle
			* BUT change the length of a slice in a method the receiver must still be a pointer
* Second is the consideration of efficiency, If the receiver is large, a big `struct` for instance, it will be much cheaper to use a pointer receiver
* Next is consistency, if some of the methods of the type must have pointer receiver, the rest should too.

__For types such as basic types, slices, and small `struct`, a value receiver is very cheap so unless the semantics of the method requires a pointer, a value receiver is efficient and clear.

[Go review comments](https://github.com/golang/go/wiki/CodeReviewComments#receiver-type)


1. if the receiver type is a `map`, `func` or `chan`, do NOT user a pointer to the. if the receiver is `slice` and the method doesn't reslice or reallocate the slice, do NOT use a pointer to it
2. If the method needs to mutate the receiver, the receiver MUST be PONTER.
3. if the receiver is a struct that contains a `sync.Mutex` (or any similar) the receiver MUST be a pointer.
4. if the receiver is a large `struct` or `arrar`, a pointer receiver is more efficient
	1. How large is large? Assume it's equivalent to passing all its elements as argument to the method. if the feels too large. it's also too large for the receiver
5. Can function or methods, either concurrently or when called from this method mutating the receiver? A value type creates a copy of the receiver when method is invoked
_____
##### References
1. [go FAQ](https://go.dev/doc/faq#different_method_sets)

