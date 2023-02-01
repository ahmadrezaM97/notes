# Go Test
Created: 2023-02-01 21:07
Tags: 
____

### Basic

```ad-quote
Tests are just code, we are in control of what we will write and how complicated we'll make our test.

We need to kepp the code quality high in our tests too.
```

what is a test?
	Repeatable steps to verify if a piece of code is working as it is supposed to.

#TODO 

### Testing with Go's `testing` package

* Golang's `testing` package supports automated testing of Go packages.
	* it exposes valuable functions that we can use to get a couple of benefits
		* a better-looking code, a standardized approach to testing, and nicer looking output.
		* Eliminate the need to create our reporting of failed/passed tests.

The `testing` package 
	1. testing
	2. benchmarking
	3. fazzitest(? new in go.18)

We use the `testing.T` type, which, when passed to `Test*` functions, manages the test state and formats the test logs.

```go
package main

import "testing"

func TestMax(t *testing.T){
	actual := Max([]int{1,2,3})
	if actual != 4 {
		t.Errorf("Expected %d, go %d",4,actual)
	}
}
```

we make the test fail by using the `t.Errorf` function and supply the error message.

__What happens when we run `go test`__

This command automates testing the package named by __the import path.__

`go test` recompiles each package along with any files with name matching the file pattern `*_test.go`

something like this
```
FAIL    github.com/ahmadrezam97/testl/sample    0.003s
```

-> make fail the test using `t.Errorf`

go test different running modes
1. The first mode is called local directory mode.
	1. this mode is active when the command is invoked with no arguments.
		1. in local directory mode, `go test` will compile the package sources and the tests found in the current directory and then run the resulting test binary.
	2. after the package test finishes, `go test` prints a summary line showing
		1. The test status (`ok` or `FAIL`)
		2. The package name
		3. The elapsed time
2. The second mode is called package list mode. the mode is activated when the command is invoked with explicit arguments.
	1. in this mode `go test` will compile and test each of the packages listed as arguments
	2. if package test passes, `go test` prints only the final `ok` summary line
	3. if a package test fails, `go test` prints the complete test output


```ad-important
title: WE HAVE TO KNOW
1. Every test file ends with `*_test.go`
2. Every test function has the format `TestXxx` 
	1. Where `Xxx` must not start with a lowercase letter.
	
```

#### Logging with `Log` and `Logf`


### Go failing tests



The most basic test

```go
func Max(numbers []int) int {
	var max int 

	for _, number := range numbers {
		if number > max {
			max = number
		}
	}
	
	return max 
}


func TestMax(numbers)
```





which kind of files?
how to run test?
what is testing.T or B or F?
what is t.Parallel?
how to mock? Once, maybe or custom config
what is subtest?
how to struct you end_to_end or integration test?


### Mocking

Mock == Substituting code  for a unit test
!= Stub  
!= Spy 
!= Fake

#### Table Test
```go
package sample_test

import (
	"testing"

	"github.com/ahmadrezam97/testl/sample"
	"github.com/stretchr/testify/assert"
)

func TestSomething(t *testing.T) {

	testTable := []struct {
		name           string
		a              int
		b              int
		expectedResult string
	}{
		{
			name:           "simple test",
			a:              1,
			b:              2,
			expectedResult: "result is 2",
		},
		{
			name:           "zero test",
			a:              0,
			b:              2,
			expectedResult: "result is 1",
		},
	}

	for _, tt := range testTable {
		// in this way you will see a beautiful subtests hierarchy
		t.Run(tt.name, func(t *testing.T) {
			t.Log("if you see this, you run verbose test or it's failed")
			actualResult := sample.DoSomething(tt.a, tt.b)
			assert.Equal(t, tt.expectedResult, actualResult)
		})
	}

}
// result 
/*
   	github.com/ahmadrezam97/testl/cmd	[no test files]
# github.com/ahmadrezam97/testl/internal/server
internal/server/server.go:30:24: s.RootHandler undefined (type *Server has no field or method RootHandler)
--- FAIL: TestSomething (0.00s)
    sample_test.go:34: if you see this, you run verbose test or it's failed
    sample_test.go:34: if you see this, you run verbose test or it's failed
    --- FAIL: TestSomething/zero_test (0.00s)
        sample_test.go:38: 
            	Error Trace:	/home/ahmadreza/Documents/personal/tamrinat/testl/sample/sample_test.go:38
            	Error:      	Not equal: 
            	            	expected: "result is 1"
            	            	actual  : "result is 0"
            	            	
            	            	Diff:
            	            	--- Expected
            	            	+++ Actual
            	            	@@ -1 +1 @@
            	            	-result is 1
            	            	+result is 0
            	Test:       	TestSomething/zero_test
FAIL
FAIL	github.com/ahmadrezam97/testl/sample	0.002s

*/
```


* if you using `sample_test` package name it forces you to only test public members.
	* it's simulate the way client may use the code, module or library
	* it's impossible to reach code, it might about your design
* if you have to
	* grow public surface


Some situation

1. I need to mock a function
	1. high order functions ( I personally don't like this)
		1. Stateless | clear
		2. Dependency graph(`sql` open outside of it's module) and ugly parameter list
	2. monkey patching ( I hate this method :| )
		1. define a package global value and reattach it for every test
			1. not good for parallel testing
2. I need to mock a method o a type
	1. use interface (e.x `io.ReadCloser` ->`*os.File`) and write a simple mock concrete type
		1.  "Accept interface, return structs"
		2. Don't be afraid to define your own interface
3. I have a large interface, I need to mock a small set of its methods
	1. use embed interface in to struct(then you have explicit satisfaction)
4. I need to mock an HTTP call
	1. parameterize URL base line and then 
	```go 
httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request){

	// mocks

}))


resp, err := makeCall(testURL)

```


_____
##### References
1.

