# GO test main func
Created: 2023-01-27 19:26
Tags: 
____

What is TestMain func exactly

No it is not to test the main function on your program. Basically the `TestMain` function provides more control over running tests.

`func TestMain(m *testing.M)`

that function will be called instead of running the tests directly.
The M struct contains methods to access and run the tests.
But there are some rules

1. You can define this function is your test file
2. Since function names need to be unique in a given package, you can only define the `Testmain` once for each package.
	1. If your package has multiple test files, choose a logic place for single `TestMain`
3. The `testing.M` struct has a single defined function named `Run()`,As you can imagine, it runs all the tests within the package.
	1. `Run` returns an exist code that can be passed to `os.Exit`
4. A minimal implementation looks like this
	1. `func Testmain(m *testing.M) { os.Exist(m.Run())}`
5. If you don't call `os.Exist` with the return code, your test command will return 0(zero), even if a test fails.


```go
package mypackagename

import (
    "testing"
    "os"
)

func TestMain(m *testing.M) {
    log.Println("Do stuff BEFORE the tests!")
    exitVal := m.Run()
    log.Println("Do stuff AFTER the tests!")

    os.Exit(exitVal)
}

func TestA(t *testing.T) {
    log.Println("TestA running")
}

func TestB(t *testing.T) {
    log.Println("TestB running")
}
```
https://medium.com/goingogo/why-use-testmain-for-testing-in-go-dafb52b406bc


_____
##### References
1.

