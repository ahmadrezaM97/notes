# Go Pointer
Created: 2022-04-15 06:37
Tags: 
____


```ad-danger
title: PASS BY VALUE
Everything in Go are pass by VALUE
```



#### BASICS

```go 

package main

import "fmt"

func main() {
	a := 42
	fmt.Println(a)
	fmt.Println(&a)

	fmt.Printf("%T\n", a)
	fmt.Printf("%T\n", &a)

	b =: &a
	fmt.Println(*b) // derefrnce 

	*b = 109
	fmt.Println(a) // 109
	/* 
		There is two diffrent type
	   	int and *int
	 	output:
			42
			0xc000018030
			int
			*int
			42
			109
	*/	
}
 
```

```ad-warning


a := 48
b := &a
c := &b ->

cannot take address of &a (value of type *int)

```

-> **Everything in Go are pass by VALUE**
-> **What you see is what you get**



#### METHOD SET

-> IMPORTANT: 
	-> The method sets determines the interface that the type implements 
		->a and the methods that can be called using a receiver of that type.

https://go.dev/play/p/uSawaFH9DqN


-> NONE-POINTER RECEIVER
		work with values that are POINTER or NONE-POINTER
-> POINTER_RECEIVER
		work only with POINTER

| Receiver | Values |
| -------- | -------|
| (t T)    | t and \*T |
| (t \*T)    | \*T       |


```ad-note
title: WHY? pinter-receiver only work with pointer?

minor:
1. Constants only exist in complie time, is not on any stack or heap, hardcoded in assebmly that we produce -> if you have pointer receiver of 'type dur int' you can not get int address

MAJOR:

VALUE SEMATNTIC CONSISTENCY
YOU ARE NOT ALLOW TO COPY A VALUE YOU CHOSE TO SHARE


```

**NOTICE THE DIFFERENCE**

```go 
import (
	"fmt"
	"math"
)

type shape interface {
	area() float64
}

type circle struct {
	r float64
}

func (c *circle) area() float64 {
	return math.Pi * c.r * c.r
}

func info(s shape) {
	fmt.Println("area", s.area())
}
func main() {
	c := circle{r: 5}
	info(c) // not work
	//fmt.Println(c.area()) // work
}
```
YOU SHOULD CHOSE A SEMANTIC FOR YOUR TYPES

#TODO SHOULD READ THIS [LINK](https://gronskiy.com/posts/2020-04-golang-pointer-vs-value-methods/#:~:text=Method%20set%20of%20a%20type%20prevents%20calling%20pointer%20method%20on%20value%20interface&text=The%20method%20set%20of%20any,the%20method%20set%20of%20T).

#TODO: SHOULD DO NINJA EXERCISE FOR POINTER

#TODO: SHOULD READ THAT ARTICLE ABOUT POINTER
https://medium.com/@meeusdylan/when-to-use-pointers-in-go-44c15fe04eac

#TODO: SHOULD READ YOUR GOOGLE DOC NOTES AND BRING THEM HERE
https://docs.google.com/document/d/1-Yd2j8qsSLN-4s3qrV5WIRDOE_CShoWNtpjsm_2PyPI/edit






___
__
##### References
1.(youtube)[https://www.youtube.com/watch?v=Z5cvLOrWlLM]

