# GO JSON
Created: 2022-09-30 19:07
Tags: 
____
https://blog.logrocket.com/using-json-go-guide/#:~:text=We%20can%20use%20the%20Marshal,comes%20with%20the%20following%20syntax.&text=It%20accepts%20an%20empty%20interface,%2C%20struct%2C%20map%2C%20etc.



### Introduction

* `JSON` is a text-based data exchange format primarily between browsers and servers.
* `JSON`( JavaScript Object Notation)

```ad-note
While sending JSPON data to a server, we do not need to necessarily convert JSON text-data to a string.

We can transport JSON data as binary data with `application/json` as `Content-Type` request header value.

A Server may handle this data appropriately by loooking at this header value
```

* `JSON` format support 6  primarily types
	* string
	* number( both signed/unsigned)
	* boolean
	* null
	* array
	* object

#### Encoding
```go
func Marshal(v interface{}) ([]byte, error)
```


```go
	data := map[string]interface{}{
			"intValue":    1234,
			"boolValue":   true,
			"stringValue": "hello!",
			"objectValue": map[string]interface{}{
				"arrayValue": []int{1, 2, 3, 4},
			},
			"nullvalue": nil,
			"complexStruct": struct {
				Name string `json:"ESM"`
				Age  int    `json:"SEN"`
			}{Name: "ahmadreza", Age: 100},
		}

	jsonData, err := json.Marshal(data, "\t" "\t")
	if err != nil {
		log.Fatalln(err)
	}
	
	fmt.Println(string(jsonData))
```


```ad-note
title: pretty JSON

MarshalIndent is like Marshal but applies Indent to format the output.
Each JSON element in the output will begin on a new line beginning with prefix
followed by one or more copies of indent according to the indentation nesting.
func MarshalIndent(v any, prefix, indent string) ([]byte, error) {


```go
jsonData, err := json.MarshalIndent(data, "***", "\t")
```

https://medium.com/rungo/working-with-json-in-go-7e3a37c5a07b

#### Marshaling
```go
func Marshal(v any) ([]byte, error) 
```

example 

```go
type MyData struct {
	name string
	age  int
}
d := MyData{name: "ahmadreza", age: 10}

b, err := json.Marshal(d)
if err != nil {
	log.Fatalln(b)
}
fmt.Println(d)
```
* Marshal returns the `JSON` encoding of v


* marshal traverse the value recursively. if an encountered value implement the marshaler interface and is not a nil pointer marshal calls its `MarshalSJON` methods to produce `JSON`
```go
// Marshaler is the interface implemented by types that
// can marshal themselves into valid JSON.
type Marshaler interface {
	MarshalJSON() ([]byte, error)
}

```

Example:
```go
type MyData struct {
	name string
	age  int
}

func (m MyData) MarshalJSON() ([]byte, error) {
	d := struct {
		Name string `json:"name"`
		Age  int    `json:"age"`
	}{Name: "Agha " + m.name, Age: 10 * m.age}

	return json.Marshal(d)
}

func example(){
	d := MyData{name: "ahmadreza", age: 10}
	b, err := json.Marshal(d)
	if err != nil {
		log.Fatalln(b)
	}
}
```


### A Danger

[[GO method set]]
### encoding and `marashling`

#### Difference of json Encoding vs Marshing

* The difference being the `Encoder` first marshals the object to a JSON encoded string, the write that data to a buffer stream -> uses more moemory overhead


* Encoding/Decoding `JSON` refers to the process of actually reading/writing the character data to a string or binary form
* Marshaling/Unmarshaling refers to the process of mapping `JSON` type from and to GO data types and primitives
* In Golang, struct data in converted into `JSON` using `Marshal()` and `JSON` data to string using `Unmarshal()` method.


### custom tag



_____
##### References
1.

