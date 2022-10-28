# Interface
Created: 2022-10-08 02:01
Tags: 
____


* Interfaces play several important roles in Go fundamentally, interface types make Go support value boxing. Consequently, through value boxing, reflection and polymorphism get supported.
* Since 1.18 Go has already supported custom generics
	* -> an interface type could be also used as type constraints
	* -> before that all interface types may be used as value type, but since then some interface which may be uses as value types are called basic interface types

```go
type somethinger inteface {
	something()
}
```


_____
##### References
1.

