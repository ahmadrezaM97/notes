# go copy
Created: 2023-01-22 06:27
Tags: 
____

The copy built-in function copies elemnts from a source slice to a destination slice
-> special case
	-> it also will copy bytes from a string to a slice of bytes

The source and destination may overlap `Copy` returns the number of element coped, which will be the minimum of len(src) and len(dst)
```go
func copy(dst, src []Type int)

  

a := []int{1, 2, 3, 4, 5, 6}
b := []int{0, 0, 0, 0, 0, 0}
copy(b, a)
fmt.Println(b) // [1,2,3,4,5,6]


a := []int{1, 2, 3, 4, 5, 6}
b := []int{0, 0}
n := copy(b, a)
fmt.Println(n, b) // 2 [1,2]


a := []int{1, 2, 3, 4, 5, 6}
b := make([]int, 0, len(a))
n := copy(b, a)
fmt.Println(n, b) // 0 []

a := []int{1, 2, 3, 4, 5, 6}
b := make([]int, len(a))
n := copy(b, a)
fmt.Println(n, b) // 6 [1,2,3,4,5,6]

```


```ad-danger
title: IT IS ABOUT LEN NOT CAP
```
_____
##### References
1.

