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


### Go Failing tests

```go
type 
func (c *T) Error(args ...any)
func (c *T) Errorf(format string, args ..any)

func (c *T) Fail()
func (c *T) FailNow()

func (c *T) Fatal(args ...any)
func (c *T) Fatalf(foramt string, args ...any)

func (c *T) Log(args ...any)
func (c *T) Logf(foramt string, args ...any)
```

`Logf` same as `fmt.Printf`
`Log` ~ `fmtPrintln`
`Logf` and `Log` save the output to the error log( instead of `os.Stdout`)

```ad-note
title: when you see result of log and logf
when the test fails or in verbose mode with `go test-v`
```


### `go test` command


`go test` is a command that automates the execution of test files and functions in a Go project.

`go test` will recompile each package and any files with names matching the file pattern `*_test.go`.

These `*_test file` can contain test functions, benchmark function and example functions(fuzz test?)

`go test` compiles test files that declare a package, ending with the suffix `*_test.go` as a separate package. it then links them with the main test binary and runs them.


#### Running Modes

`go test` has two running modes.

1. __Local directory mode, or running without arguments.__
	1. `go test` compiles the package sources and tests found in the current directory and the runs the resulting test binary.
	2. this mode __disables caching__.
	3. After the package test finishes, `go test` prints a summary line showing 
		1. The test status (`ok` and `FAIL`)
		2. The package name
		3. Elapsed time.
2. __Package list mode, or running with arguments.__
	1.  `go test` compiles and tests each package listed as arguments to the command.
	2. If a package test passes, `go test` prints only the final `ok` summary line.
	3. If a package test fails, `go test` prints the complete test output.
	4. To run your test in this mode, run `go test` with explicit package arguments.
		1. `go test PACKAGE_NAME` to test a specific package 
		2. `go test ./...`  to test all packages in directory tree
		3. `go test .` to run all tests in the current directory

```ad-note
title: verbose mode
If we would like to see a more detailed output, we can use the `-v` flag
```
```bash
$ go test ./person -v 
=== RUN   TestNewPersonPositiveAge
--- PASS: TestNewPersonPositiveAge (0.00s)
=== RUN   TestNewPersonNegativeAge
--- PASS: TestNewPersonNegativeAge (0.00s)
PASS
ok  	github.com/ahmadrezam97/testl/person	(cached)

```


```ad-note
by adding the `-failfast` flag, we stopped wight where we got the first failure
```



#### Test Coverage

Another exciting feature that is packed in `go test` is test coverage.
Taken from Go's website:

```ad-quote
Test coverage is a term that describes how much of a package's code is exercised by running the package's test.
If executing the test sute causes 80% of the package's source statements to be run, we sa the coverage is 80%
```

```bash
$ go test ./... --cover
ok  	github.com/ahmadrezam97/testl/person	(cached)	coverage: 80.0% of statements
```


```bash
$go test ./person --coverprofile=prof.out
ok  	github.com/ahmadrezam97/testl/person	0.001s	coverage: 83.3% of statements
```


```bash
$ cat prof.out
mode: set
github.com/ahmadrezam97/testl/person/person.go:9.42,10.13 1 1
github.com/ahmadrezam97/testl/person/person.go:14.2,14.15 1 1
github.com/ahmadrezam97/testl/person/person.go:18.2,20.8 1 1
github.com/ahmadrezam97/testl/person/person.go:10.13,12.3 1 1
github.com/ahmadrezam97/testl/person/person.go:14.15,16.3 1 0
github.com/ahmadrezam97/testl/person/person.go:23.44,25.2 1 1
```


```bash
$ go tool cover -html=prof.out
```

![[go-cover.png]]


### using `-short` mode

The `-shot` mode for `go test` allows us to mark any long-running tests to be skipped in this mode.

`go test` and the `testing` package support this via the `t.Skip()`, the `testing.Short()` function and `-short` flag

```bash
$ go test ./... -short -v 
```

```
=== RUN   TestSomething
=== RUN   TestSomething/simple_test
    sample_test.go:35: if you see this, you run verbose test or it's failed
=== RUN   TestSomething/zero_test
    sample_test.go:35: if you see this, you run verbose test or it's failed
--- FAIL: TestSomething (0.00s)
    --- PASS: TestSomething/simple_test (0.00s)
    --- FAIL: TestSomething/zero_test (0.00s)
=== RUN   TestFoo
    sample_test.go:51: SHIIIIIIIIIIT
--- SKIP: TestFoo (0.00s)
FAIL
FAIL	github.com/ahmadrezam97/testl/sample	0.002s
FAIL

```


```go
func TestFoo(t *testing.T) {
	if testing.Short() {
		t.Skipf("SHIIIIIIT")
	}
	t.Logf("Testing foo")
	t.FailNow()
	t.Logf("Another log from foo")
}
```


### `Subtests`

#### introduction

In Go 1.7, the testing package introduces a Run method on the `T` and `B` types that allows for the creation of `subtests` and `sub-benchmarks`.

it enables better handling of failures, fine-grained control of which tests to run from the command line, control of parallelism, and often results in simpler and more maintainable code.


### `t.Run` method

```go 
func (t *T) Run(name string, f func(t *T)) bool
```

`Run` runs the function in a separate goroutine and blocks until it returns or calls `t.Parallel` to become a parallel test.



```go
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

		t.Run(tt.name, func(t *testing.T) {
			t.Log("if you see this, you run verbose test or it's failed")
			actualResult := sample.DoSomething(tt.a, tt.b)

			if tt.expectedResult != actualResult {
				t.FailNow()
				t.Errorf("")
			}

			assert.Equal(t, tt.expectedResult, actualResult)
		})
	}

}
```

```
$ testl go test ./person -v
=== RUN   TestOlder
=== RUN   TestOlder/FirstOlderThanSecond
=== RUN   TestOlder/SecondOlderThanFirst
--- PASS: TestOlder (0.00s)
    --- PASS: TestOlder/FirstOlderThanSecond (0.00s)
    --- PASS: TestOlder/SecondOlderThanFirst (0.00s)
=== RUN   TestNewPersonPositiveAge
--- PASS: TestNewPersonPositiveAge (0.00s)
=== RUN   TestNewPersonNegativeAge
--- PASS: TestNewPersonNegativeAge (0.00s)
=== RUN   TestNewPersonWithMoreThan130Years
--- PASS: TestNewPersonWithMoreThan130Years (0.00s)
PASS
ok  	github.com/ahmadrezam97/testl/person	0.001s

```


#### Selectively running subtests with `go test`

```bash
go test -v -count=1 -run="TestOlder/FirstOlderThanSecond"
```

#### Shared Setup and Teardown

another somewhat hidden side of subtests is unlocking the ability to create isolated setup and teardown functions

```go
func setupSubtest(t *testing.T) {
	t.Logf("[SETUP] hello 👋")
}

func tearDownSubtest(t *testing.T) {
	t.Logf("[TEARDOWN] Bye, bye")
}

func TestOlder(t *testing.T) {

	cases := []struct {
		name     string
		age1     int
		age2     int
		expected bool
	}{
		{
			name:     "FirstOlderThanSecond",
			age1:     1,
			age2:     2,
			expected: false,
		},
		{
			name:     "SecondOlderThanFirst",
			age1:     2,
			age2:     1,
			expected: true,
		},
	}

	for _, c := range cases {
		t.Run(c.name, func(t *testing.T) {
			setupSubtest(t)
			defer tearDownSubtest(t)

			p1, _ := person.NewPerson(c.age1)
			p2, _ := person.NewPerson(c.age2)

			got := p1.Older(p2)

			if got != c.expected {
				t.Errorf("Expected %v > %v, got %v", p1.Age(), p2.Age(), got)
			}
		})
	}
}
```

```
go test ./person -v
=== RUN   TestOlder
=== RUN   TestOlder/FirstOlderThanSecond
    person_test.go:10: [SETUP] hello 👋
    person_test.go:14: [TEARDOWN] Bye, bye
=== RUN   TestOlder/SecondOlderThanFirst
    person_test.go:10: [SETUP] hello 👋
    person_test.go:14: [TEARDOWN] Bye, bye
--- PASS: TestOlder (0.00s)
    --- PASS: TestOlder/FirstOlderThanSecond (0.00s)
    --- PASS: TestOlder/SecondOlderThanFirst (0.00s)

```


#### TestMain

There are times when our test file has to do some extra setup or teardown before or after the tests in a file are run.

Therefore, when a test file contains a `TestMain` function, the test will call `TestMain(m *testing.M)` instead of running the tests directly.

Think of it in this way
	every test file contains a "hidden" `TestMain` function, and its contents look something like this

```go
func TestMain(m *testing.M) {
	os.Exist(m.Run())
}
```

`TestMain` will run in the main goroutine, and it does the setup or teardown necessary arround a call to `m.Run`.

`m.Run` runs all the test functions in the test file.

`Testmain` will take the result of the `m.Run` invcation and then call `os.Exit` with the result as an argument.



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

