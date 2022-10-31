# GO JSON
Created: 2022-09-30 19:07
Tags: 
____
https://blog.logrocket.com/using-json-go-guide/#:~:text=We%20can%20use%20the%20Marshal,comes%20with%20the%20following%20syntax.&text=It%20accepts%20an%20empty%20interface,%2C%20struct%2C%20map%2C%20etc.


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






_____
##### References
1.

