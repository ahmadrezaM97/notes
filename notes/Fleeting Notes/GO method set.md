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

what does it mean?
```go
type Data struct {}

func (d *Data)DoPointer(){}
func (d Data)DoValue(){}

d := Data{}
// `T` has just DoValue, and `*T` has both Dovalue and DoPointer
d.DoPointer() // ok
d.DoValue()// not ok

(&d).DoPointer() // ok
(&d).DoValue() // ok

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



_____
##### References
1. [go FAQ](https://go.dev/doc/faq#different_method_sets)

