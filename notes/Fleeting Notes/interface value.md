Created: 2022-12-07 12:45
Tags: 
____

# duck typing 

```ad-quote
If it quacks, moves and swims like a duck then it should be a duck.
```

If it has a set of methods that match an interface, then you can use it wherever that interface is needed without explicitly defining that you types implement that interface.


Languages typically fall into one of two comps 
* prepare tables for all the method calls statically ( as c++ or java)
* do method lookup at each call( java script and python included) and add fancy caching to make that call efficient.

GO sits halfway between the two, it has method table but compute them at run time.

[[ make sure a type implement an interface ]]

Example 
```go 
type Binary int

func (b Binary) String() string {
	return strconv.Itoa(int(b))
}

type Stringer interface {
	String() string
}

func main() {
	var b Stringer
	fmt.Printf("%T %v\n", b, b) // <nil> <nil>
	a := Binary(10)
	b = a
	fmt.Printf("%T %v\n", b, b) // main.Binary 10
}

```

![[gointer1.png]]


Interface values are represented as a two-word pair 
*  giving a pointer to information about the type stored in the interface and 
* a pointer to the associated data

> these pointer are implicit, not directly exposed to GO





![[gointer2.png]]


_____
##### References
1.

