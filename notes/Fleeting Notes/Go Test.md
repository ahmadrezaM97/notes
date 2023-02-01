# Go Test
Created: 2023-02-01 21:07
Tags: 
____

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
2. I need to mock a method o a type
3. I have a large interface, I need to mock a small set of its methods
4. I need to mock an HTTP call

_____
##### References
1.

