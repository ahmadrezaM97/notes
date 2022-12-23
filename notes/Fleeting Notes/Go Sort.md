# Go Sort
Created: 2022-12-23 22:06
Tags: 
____
```go

type Interface interface {
	Len() int
	Less(i, j int) bool
	Swap(i, j int)
}

```

```go
type IntSlice []int

func (x IntSlice) Len() int           { return len(x) }
func (x IntSlice) Less(i, j int) bool { return x[i] < x[j] }
func (x IntSlice) Swap(i, j int)      { x[i], x[j] = x[j], x[i] }

// Sort is a convenience method: x.Sort() calls Sort(x).
func (x IntSlice) Sort() { Sort(x) }

type StringSlice []string

func (x StringSlice) Len() int           { return len(x) }
func (x StringSlice) Less(i, j int) bool { return x[i] < x[j] }
func (x StringSlice) Swap(i, j int)      { x[i], x[j] = x[j], x[i] }

// Sort is a convenience method: x.Sort() calls Sort(x).
func (x StringSlice) Sort() { Sort(x) }

```

```go
type reverse struct {
	// This embedded Interface permits Reverse to use the methods of
	// another Interface implementation.
	Interface
}

// Less returns the opposite of the embedded implementation's Less method.
func (r reverse) Less(i, j int) bool {
	return r.Interface.Less(j, i)
}

// Reverse returns the reverse order for data.
func Reverse(data Interface) Interface {
	return &reverse{data}
}
```

```go
// IsSorted reports whether data is sorted.
func IsSorted(data Interface) bool {
	n := data.Len()
	for i := n - 1; i > 0; i-- {
		if data.Less(i, i-1) {
			return false
		}
	}
	return true
}
```

```go
package main

import (
	"fmt"
	"sort"
)

func IntSliceSort() {
	a := []int{1, 3, 2, 4}
	sort.Ints(a)
	fmt.Println(a)
}

func StringSliceSort() {
	a := []string{"a", "bbb", "aa", "b"}
	sort.Strings(a)
	fmt.Println(a)
}

type employee struct {
	age    int
	salary int
}

type employees []employee

func (e employees) Less(i, j int) bool {
	return e[i].age < e[j].age
}

func (e employees) Len() int {
	return len(e)
}

func (e employees) Swap(i, j int) {
	e[i], e[j] = e[j], e[i]
}

func CustomTypeSort() {
	emps := []employee{
		{age: 4, salary: 17},
		{age: 1, salary: 20},
		{age: 5, salary: 16},
		{age: 3, salary: 18},
		{age: 2, salary: 19},
	}
	sort.Sort(employees(emps))
	fmt.Println(emps)
}

func CustomSort() {
	emps := []employee{
		{age: 25, salary: 10},
		{age: 1, salary: 0},
		{age: 25, salary: 14},
		{age: 26, salary: 13},
		{age: 27, salary: 12},
	}

	sort.Slice(
		emps,
		func(i, j int) bool {
			return emps[i].salary < emps[j].salary
		},
	)
	fmt.Println(emps)
	// also see stableSlice
	//sort.StableSlice()
}

func Reverse() {
	a := []int{1, 2, 3, 4}
	sort.Sort(sort.Reverse(sort.IntSlice(a)))
	fmt.Println(a)
}

func ReverseCustomType() {
	emps := []employee{
		{age: 25, salary: 10},
		{age: 1, salary: 0},
		{age: 25, salary: 14},
		{age: 26, salary: 13},
		{age: 27, salary: 12},
	}

	sort.Slice(
		sort.Reverse(employees(emps)),
		func(i, j int) bool {
			return emps[i].salary < emps[j].salary
		},
	)
	fmt.Println(emps)
}

func main() {
	CustomTypeSort()
	CustomSort()
	Reverse()
	ReverseCustomType()
}

```


_____
##### References
1.

