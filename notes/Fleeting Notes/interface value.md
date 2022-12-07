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

##### The first word 
* The first word in the interface value points at what I call an interface table or itable.
* Itable begins with some metadata about the types involved and then becomes a list of function pointers.
	-> Interface type, not the dynamic type.


#### The Second word 
* The second word in the interface value points at the actual data, in this case a copy of b. 
* The assignment var s Stringer = b make a copy of b rather than point at b.
	* if b later changes s won't
* Values stored in interface might be arbitrarily large, but only one word is dedicated ti holding the value in the interface  structure, -> so the assignment allocates a chink of memory on the heap and records the pointer in the one-word slog ( there's an obvious optimization when the value does fit in the slot)

```ad-note 
title: type switch
The Go compiler generates code equivalent to the C expression s.tab->Type to Obtain the type pointer and check it agints the desire type.

It the types match the value can be copied by dereferencing s.data
```

```ad-note 
title: function call

To call s.String(), the Go compiler generate code that does equivalent of the C expression s.tab->fun[0](s.data)

it calls the appropriate function pointer from the itable, passing the interface value's data word as the function's first
```


#### Memory optimizations

![[]]

_____
##### References
1.

