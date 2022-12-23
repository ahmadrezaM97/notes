Created: 2022-12-23 18:34
Tags: 
____

#### Why is my nil error value not equal to nil?

Under The covers, interfaces implemented as two elements, a type T and a value V.
V is a concrete value such as an int,struct or pointer, never interface itself,  and has type T. for instance, if we store the int value 3 in an interface, the result would be (T=int, V=3)

```ad-important
Ani interface value is `nil` only if the V and T both unset (T=nil, V is on set)
```

```go
package main

import "fmt"

type CustomError struct {
	msg string
	err error
}

func (c *CustomError) Error() string {
	return fmt.Sprintf("%s: %v", c.msg, c.err)
}

func ReturnCustomErrorAsErrorInterface() error {
	var err *CustomError
	return err
}

func ReturnCustomErrorNilPointer() *CustomError {
	var err *CustomError
	return err
}

func ReturnCustomErrorNoneNilPointer() *CustomError {
	var err CustomError
	return &err
}

func main() {
	fmt.Println(ReturnCustomErrorAsErrorInterface() == nil) // false
	fmt.Println(ReturnCustomErrorNilPointer() == nil)       // true
	fmt.Println(ReturnCustomErrorNoneNilPointer() == nil)   // false
}
```
_____
##### References
1.

