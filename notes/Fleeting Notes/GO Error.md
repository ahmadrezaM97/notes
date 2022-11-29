# GO Error
Created: 2022-11-29 23:38
Tags: 
____


Error interface

```go
type error interface {
	Error() string 
}
```

### Custom error


sometimes a custom error is the cleanest way to capture detailed error information.

for example
_____
##### References
1.

