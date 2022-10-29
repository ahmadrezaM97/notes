# Basic Data Types
Created: 2022-10-30 00:00
Tags: 
____
* data types 
	* basic types
		* numbers
		* strings
		* boolean
	* aggregate types
		* array
		* slice
		* map
		* struct
	* reference types 
		* pointers
		* slice
		* maps
		* function
		* channels
	* interface types


#### int vs int32 vs int 64

int is a proper type for length of an array based on the platform

type Rune `int32`
type byte `uint8`

`uintptr` is sufficient to hold all the bits f a pointer value.
```go
  
a := 0666
fmt.Println(a) // 438

```
### overflow

```go
var u uint8
fmt.Println(u+1, u*u) // 0, 1 
```


``` go
func Int32Inc(i int32) int32 {
	if i == math.MaxInt32 {
		panic("int32 overflow")
	}
	return i + 1
}

func AddInt(a, b int) int {
	if a > math.MaxInt-b {
		panic("int overflow")
	}

	return a + b
}
```


```go
fmt.Printf("%08b\n", 10) // "00001010"

```

#### Floating-point

``` go
fmt.Printf("%8.3f\n", f)

```


_____
##### References
1.

