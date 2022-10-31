# Go Map
Created: 2022-10-30 03:10
Tags: 
____
basic operations

```go 
// construct 
m := map[int]string

// lookup
v = m[2]

//delete
delete(m,1)

//iterate
for k,v := range m {
	fmt.Println(k,v)
}

//size
len(m)

```

```ad-important
Key type must have an == operation ( no maps, slices, or funcs as keys)

slice and map both could refrence to themslevs.

```
```go
a := make(map[int]any)
a[0] = a

a := []any{nil,nil}
a[0] = a
```



___
__
##### References
1.

