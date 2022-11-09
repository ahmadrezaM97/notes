# Nil
Created: 2022-10-22 19:25
Tags: 
____

#### Zero Value

* `bool` -> `false`
* `numbers` -> `0`
* `string` -> `""`
* `pointer, slice, map, channels, functions, interface `-> `nil`

```ed-note
title: https://go.dev/ref/spec
unless the value is the predeclared identifier nil, which has no type
```

```ad-note

nil is a predeclared identifier representing the zero value for a `pointer`, `channel`, `func`, `interface`, `map` or `slice` type.

```

>nil is not a keyword


#### pointer
pointers in Go
* the point to a position in memory
* similar to C or C++ but
	* no pointer arithmetic -> memory safety
	* garbage collection

#### Slice
* nil slice doesn't have backking array [ptr: nil, len:0, cap:0]

### interface
* interface has to thing dynamic type and dynamic value

```go
var s fmt.Stringer // Stringer (nil, nil)
fmt.Println(s == nil) // true
```

> (nil,nil) equal nil


The real WTF:
```go
	var p *Person
	var s fmt.Stringer
	s = p // Stringer(*Pointer, nil)
	fmt.Println(s == nil) // false
	fmt.Println(p == nil) // true
```

SO when is nil not nil?

```go
type doError struct{}

func (d *doError) Error() string { return "" }

func do() error {
	var err *doError
	return err
}

func main() {
	err := do()
	fmt.Println(err == nil)
}
```

```go
type doError struct{}

func (d *doError) Error() string { return "" }

func anotherDo() *doError {
	return nil
}

func WrapDo() error {
	return anotherDo()
}

func main() {
	err := WrapDo()
	fmt.Println(err == nil) // false
}
```

do not declare concrete error type
do not return concrete error type

#### kinds of `nil`

* `pointer` -> point to nothing
* `slice` -> have no backing array
* `maps` -> are not initialized
* `channels` -> are not initialized
* `functions` -> are not initialized
* `interfaces` -> have no value assigned, not even a nil pointer

```go

// keep nil useful

func (t *tree)Find(v int) bool {
	if t == nil {
		return false
	}

	return t.v == v || t.l.Find(v) || t.r.Find(v)
}
```


### slice
```go

var s []slice
s == nil // true
len(s) // 0
cap(s) // 0

for range s // itertes zero time

s[10] // panic
// panic: runtime error: index out of range [10] with length 0


var s []string
for i := 0; i < 10; i++ {
	fmt.Printf("len: %d cap %d \n", len(s), cap(s))
	s = append(s, "str")
}

// len: 0 cap 0  //s is nil
// len: 1 cap 1  // allication
// len: 2 cap 2  // reallocation
// len: 3 cap 4  // reallocation
// len: 4 cap 4  
// len: 5 cap 8   // reallocation
// len: 6 cap 8 
// len: 7 cap 8 
// len: 8 cap 8 
// len: 9 cap 16   // reallocation
	
```

### maps

```go

var m map[string]int

len(m) // 0

for k,v := range m // iterates zero times

v, ok := m["key"] // 0(zero_value) false

m["key"] = 10 // panic
// assigment to entry in nil map
```


nil is valid value for maps, readonly empty map

### Channel

closed channel
```go
cc := make(chan string)
close(cc)
close(cc) // panic: close of closed channel
v := <-cc // zero value return
fmt.Printf("%q\n", v) /
cc <- "S" // send on closed chennel


```
nil channel
```go

var c chan int

c == nil // true
v, ok <- c // block
c <-10    // block 
close(c) //panic: close of nil channel

```
_____
##### References
1.

