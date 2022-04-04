# GO web-server
Created: 2022-04-01 18:42
Tags: 
____

### Overview
- HTTP request
	- request line
	- headers
	- optional message body
- Request-Line
	- Method SP Request-URL SP HTTP-Version CRLF
	- examples
		- GET /path/index.html HTTP/1.1
		- GET / HTTP/1.1

- HTTP Response
	- status line
	- header
	- optional message body
- Status-Line
	- example status line:
		- HTTP/1.1 200 ok

- [ ] should read a about HTTP request and response ( [[RFC]])


to write a HTTP server, we should make a TCP server first and then we build a HTTP server on top of that.
RFC 7230

##### We use /net package in standard library

[net.Listen](https://godoc.org/net#Listen)

```go 
func Listen(net, addr string)(Listener, error)
```

#### Listener
[net.Listener]()

```go
type Listener interface {

	// Accept waits for and returns the next connection to the listener.
	Accpet() (Conn, error)

	// Close closes the listener.
	// Any Blocked Accept operation will be unblocked and return errors.
	Close() err

	//Adddr return the listener's network address
	Addr() Addr
}
```

#### Connection
[net.Conn]()

```go 

type Conn interface {

	Read(b []bytes)(n int, err error)
	Write(b []bytes)(n int, err)


	Close() error


	LocalAddr() Addr
	RemoteAddr() Addr

	SetDeadLine(t time.Time) error
	SetReadDeadLine(t time.Time) error
	SetWriteDeadLine(t time.Time) error

}
```



Dial
[net.Dial]()

```go 

func Dial(network, address string)(Conn, error)

```

Write

[io.WriteString]()

```go
func WriteString(w Writer, s string)(n int, err error)
```

fmt.Fprint
```go 

func Fprintln(w io.Writer, a ...interface{}) (n int, err error)

```


_____
##### Refrences
1.

