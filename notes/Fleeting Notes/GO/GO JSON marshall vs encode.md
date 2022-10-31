# GO JSON marshall vs encode
Created: 2022-09-29 22:18
Tags: 
____

#### Difference of json Encoding vs Marshing

* the differece being the `Encoder` first marshals the object to a JSON encoded string, the write that data to a buffer stream -> uses more memody overhead


* Encoding/Decoding `JSON` refers to the process of actually reading/writing the character data to a string or binary form
* Marshaling/Unmarshaling refers to the process of mapping `JSON` type from and to GO data types and primitives
* In Golang, struct data in converted into `JSON` using `Marshal()` and `JSON` data to string using `Unmarshal()` method.



_____
##### References
1.

