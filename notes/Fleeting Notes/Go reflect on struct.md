# Go reflect on struct
Created: 2022-12-27 17:22
Tags: 
____


```go
package main

import (
	"fmt"
	"reflect"
)

type Person struct {
	Name string `validation:"maxlen=2"`
	Age  int    `validation:"min=0,max=100"`
}

func (p Person) DoSomethingValueReceiver() {
	fmt.Println("something :D")
}

func (p *Person) DoSomethingPinterReceiver() {
	fmt.Println("something :D")
}

func main() {
	p := Person{Name: "ahmadreza", Age: 25}

	printFields(p)
	printMethods(p)
	printTags(p)
}

func printFields(p Person) {
	pv := reflect.ValueOf(p)
	pt := reflect.TypeOf(p)

	for i := 0; i < pv.NumField(); i++ {
		fmt.Printf("filed name=%s, value=%v\n",
			pt.Field(i).Name, pv.Field(i),
		)
	}
}

func printMethods(p Person) {
	pt := reflect.TypeOf(p)
	pv := reflect.ValueOf(p)

	for i := 0; i < pt.NumMethod(); i++ {
		fmt.Printf("method of Person(T) name = %s\n", pt.Method(i).Name)

		pv.Method(i).Call([]reflect.Value{})

	}

	ppt := reflect.TypeOf(&p)
	for i := 0; i < ppt.NumMethod(); i++ {
		fmt.Printf("method of *Person(*T) name = %s\n", ppt.Method(i).Name)
	}
}

func printTags(p Person) {
	pv := reflect.ValueOf(p)

	for i := 0; i < pv.NumField(); i++ {
		field := reflect.TypeOf(p).Field(i)
		fmt.Printf("name = %s, tag_validation= %s\n",
			field.Name,
			field.Tag.Get("validation"),
		)
	}
}

```
_____
##### References
1.

