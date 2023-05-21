# Defer Panic Recover
Created: 2023-01-06 18:42
Tags: 
____

#### Defer panic recover
https://go.dev/blog/defer-panic-and-recover

Defer statements allow us to think about closing each file right after opening it, guaranteeing that, regardless of the number of return statements in the function, the files will be closed.

```ad-note
title: rule 1
1. A deffered function's arguments are evaluated when the defer statement is evalueted
```
```go

package main

import "fmt"

func Do() {
	value := 0
	defer fmt.Println(value) // value is zero(0)

	value = 19
}
func main() {
	Do()
}
```

```
0
```

```ad-note 
title: rule 2
2. Deffered function calls are executed in Last in First Out order after the surreounding function returns.
```


```go
package main

import "fmt"

func Do() {
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)

	panic("panic")

	defer fmt.Println(4)
}
func main() {
	Do()
}
```

```
3
2
1
panic: panic

goroutine 1 [running]:
main.Do()
	/home/ahmadreza/Documents/personal/tamrinat/goclass/errorl/main.go:10 +0xf4
main.main()
	/home/ahmadreza/Documents/personal/tamrinat/goclass/errorl/main.go:15 +0x17
exit status 2
```


```ad-note 

3. Deferred functions may read and assign to the returning function's named return values
```
```go
package main

import (
	"errors"
	"fmt"
)

func Do() (err error) {

	defer func() {
		err = errors.New("defer error")
	}()

	err = errors.New("normal error")

	return err
}

func main() {
	if err := Do(); err != nil {
		fmt.Println(err)
	}
}
```

```
defer error
```


__Panic__ is a built-in function that stops the ordinary flow of control and begins panicking. when the function F calls panic, execution of F stops, any deferred functions in F are executed normally and then F returns to it's caller. To the caller, F then behaves like a call to panic.
The process continues up the stack until all functions in the current have returned
panic can be initiated by invoking panic directly. they can also be caused by runtime errors, such as out-of-bounds array accesses.

__Recover__ is built-int function that regains control of a panicking Goroutine. Recover is only useful inside deferred functions
During normal execution, a call to recover will return nil and have not other effect.
If the current goroutine i panicking, a call to recover will capture the value given to panic and resume normal execution.

#TODO 
add example codes



_____
##### References
1.

