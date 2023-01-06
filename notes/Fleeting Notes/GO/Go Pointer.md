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


### Pointer of pointer 

```go
func main() {
	a := 10
	b := &a
	c := &b
	d := &c
	fmt.Println(d)
}

```
[[GO method set]]