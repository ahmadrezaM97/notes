# GO JSON
Created: 2022-09-30 19:07
Tags: 
____
https://blog.logrocket.com/using-json-go-guide/#:~:text=We%20can%20use%20the%20Marshal,comes%20with%20the%20following%20syntax.&text=It%20accepts%20an%20empty%20interface,%2C%20struct%2C%20map%2C%20etc.

### Introduction

* `JSON` is a text-based data exchange format primarily between browsers and servers.
* `JSON`( JavaScript Object Notation)

```ad-note
While sending JSON data to a server, we do not need to necessarily convert JSON text-data to a string.

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

#### Marshal

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

	jsonData, err := json.Marshal(data)
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


#### default encoding types

| Go type | JSON Type |
|---------|-----------|
| bool | boolean|
| floaf64 | number|
|string|string|
|nil pointer| null|
| time.Time | RFC 339|


#### JSON Tag

* `omitempty`
	* when unmarshling data you can leave out a key completely if the key's value contaons a zero using the `omitempty` tag.
* non-exported(lower-case) fields are ignored by the marshaled
* ignore field explicitly
	* using `json:"-"` you can ignore exported fields explicitly

example
```go
func DoJsonTag() {

	type Data struct {
		Name    string `json:"name"`
		Family  string `json:"family"`
		Phone   string `json:"-"`
		Email   string
		Address string `json:"address,omitempty"`
	}

	sample := Data{
		Name:  "ahmadreza",
		Phone: "09374078185",
		Email: "ahmad1997rezamarashi",
	}

	var inp []byte
	if d, err := json.Marshal(sample); err != nil {
		log.Fatalln(err)
	} else {
		inp = d
		fmt.Printf("%+v\n", string(d))
		// look these is no address here because we use emitempty tag here but family is here with empty straing
		// {"name":"ahmadreza","family":"","Email":"ahmad1997rezamarashi"}
	}

	var data Data

	if err := json.Unmarshal(inp, &data); err != nil {
		log.Fatalln(err)
	} else {
		fmt.Printf("%+v\n", data)
		// phone is empty because we set json tag "-"
		// {Name:ahmadreza Family: Phone: Email:ahmad1997rezamarashi Address:}
	}

}

```

#### The Marshaler (custom marshaling)

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


```ad-warning
title: Consider Method Set 

1. if you define `MarshalJSON` method as a pointer receiver, you it would blong to *Mydata then Mydata itself does not satisfy the `Marshaler` interface. 

2. Since you don't want to mutate receiver in the `MarshalJSON`, its better to define it as a value receiver so it blongs to both MyData and *Mydata

3. for understanting method set check 
[[GO method set]]

```



#### Unmarshal

```go
func Unmarshal(data []byte, v any) error
```

* Unmarshal parses the JSON-encoded data and stores the result in the value pointer to by v
* If v is nil or not a pointer, `Unmarshal` returns an `InvalidUnimarshalError`

example:

```go
var d = `{ 
	"name": "ahmadreza", 
	"age": 18
}`

type Data struct {
	Name   string `json:"name"`
	Age    int    `json:"age"`
	Number string `json:"number,omitempty"`
}

func Run() {

	var v Data
	if err := json.Unmarshal([]byte(d), &v); err != nil {
		log.Fatalln(err)
	}
	
	fmt.Printf("%+v\n", v)
}
```

### The Unmarshler (custom unmarshling)

```go
type Unmarshaler interface {
	UnmarshalJSON([]byte) error
}
```

example

```go

type User struct {
	Name string `json:"-"`
	Age  int    `json:"age"`
}

func (m *User) UnmarshalJSON(data []byte) error {
	var d struct {
		FirstName string `json:"firstname"`
		LastName  string `json:"Lastname"`
		Age       int    `json:"age"`
	}

	if err := json.Unmarshal(data, &d); err != nil {
		return err
	}

	m.Name = fmt.Sprintf("%s - %s", d.FirstName, d.LastName)
	m.Age = d.Age

	return nil
}

var d = `{ 
	"firstname": "ahmadreza", 
	"lastname": "marsahi",
	"age": 18
}`

func Run() {

	var u User
	if err := json.Unmarshal([]byte(d), &u); err != nil {
		log.Fatalln(err)
	}

	fmt.Printf("%+v\n", u)
}

```

```ad-warning
title: consider method set

if you define `UnmarshalJSON` as value reciever it could mutate receiver itself, it MUST be a pointer receiver.

for better understanding check [[GO method set]]
```


### `Encoding` vs `marashling`

1. The difference being the `Encoder` first marshals the object to a `JSON` encoded string, the write that data to a buffer stream -> uses more memory overhead
2. Encoding/Decoding `JSON` refers to the process of actually reading/writing the character data to a string or binary form
3.  Marshaling/Unmarshaling refers to the process of mapping `JSON` type from and to GO data types and primitives
4. In GO, struct data in converted into `JSON` using `Marshal()` and `JSON` data to string using `Unmarshal()` method.

```ad-summary
title: encoding vs marashling

encoding -> marshal object to binary and write it to io.Writer 
marshaling -> marshal object to binery and pass it

```go
func Marshal(v any) ([]byte, error)
// vs
func NewEncoder(w io.Writer) *Encoder
func (enc *Encoder) Encode(v any) error

```

```go

func NewEncoder(w io.Writer) *Encoder {
	return &Encoder{w: w, escapeHTML: true}
}

func (enc *Encoder) Encode(v any) error

```

###### Encoding example

```go 
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func Run() {
	u := User{Name: "ahmadreza marashi", Age: 25}
	if err := json.NewEncoder(os.Stdout).Encode(u); err != nil {
		log.Fatalln(err)
	}
	/*
		{"name":"ahmadreza marashi","age":25}
	*/
}

func Run2(){
	enc := json.NewEncoder(os.Stdout)
	u := User{Name: "ahmadreza marashi", Age: 25}
	enc.SetIndent("", "\t")
	_ = enc.Encode(u) // here we just ignore the potential errors
	_ = enc.Encode(u)
	/*
		{
	        "name": "ahmadreza marashi",
	        "age": 25
		}
		{
		        "name": "ahmadreza marashi",
		        "age": 25
		}
	*/
}

```


#### Decoding vs Unmarshaling

```go
func NewDecoder(r io.Reader) *Decoder {
	return &Decoder{r: r}
}


func (dec *Decoder) Decode(v any) error 
```

* Decode reads the next `JSON-encoded` value from its input and stores it in the value pointed to by v

example
``` go
func DoDecoder() {
	data := []byte(`
	{
		"user":"ahmadrezam97",
		"reqs": ["r1", "r2"],
		"info": {
			"name": "ahmadreza",
			"age": 25
		}
	}
	`)
	reader := bytes.NewReader(data)
	betterValue := struct {
		User string   `json:"user"`
		Reqs []string `json:"reqs"`
		Info struct {
			Name string `json:"name"`
			Age  int    `json:"age"`
		} `json:"info"`
	}{}
	dec := json.NewDecoder(reader)
	if err := dec.Decode(&betterValue); err != nil {
		log.Fatalln(err)
	}
	fmt.Println(betterValue)
}

```




### Custom tag
[[Go struct custom tag]]


_____
##### References
1.

