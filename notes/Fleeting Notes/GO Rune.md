# GO Rune
Created: 2022-09-30 20:37
Tags: 
____

#### What is a string?

* In Go, a string is in effect a read-only slice of bytes.
* It's important to state right up front that a string hold `arbitrary` bytes.
	* It is not required to hold Unicode text, `UTF-8` text, or any other predefined format.
	* As far as the content of a string is concerned, it is exactly equivalent to a slice of bytes.

### printing a weird slice if bytes as a strings

Because some of the bytes in out sample string are not valid ASCII, not even valid `UTF-8`, print the string will produce ugly output.
```go
	const sample = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"
	fmt.Println(sample)
	// ��=� ⌘
```

### Rune 

please first check this document [[Unicode, UTF-8, ASCII]]

* A `rune` is an alias to the `int32` data type.
* `rune` It represent a Unicode code point.
* Go source code us `UTF-8`.
* `String` can contain `Unicode` text encoded in `UTF-8`, which encodes all Unicode points using one to four bytes.
* A string is a sequence of bytes; more precisely, a slice of arbitrary bytes.

```go
type rune = int32
```

#### rune
```go
    a1 := '🧺'
    a2 := '\u2665'
    a3 := '\U0001F3A8'

    fmt.Printf("%c\n", a1) // 🧺
    fmt.Printf("%c\n", a2) // ♥
    fmt.Printf("%c\n", a3)// 🎨 
    a1 := '🧺'
	
	s := "a🐧b🐧c🐧d"
	fmt.Printf("%U\n", []rune(s)) // [U+0061 U+1F427 U+0062 U+1F427 U+0063 U+1F427 U+0064]
```


#### Difference between byte and rune
```go 
	s := "آ a"
	fmt.Println([]rune(s)) // [1570 32 97]
	fmt.Println([]byte(s)) // [216 162 32 97]
```


#### String to rune
```go
	a := "ahmadreza"
	runeArr := []rune(a)
	fmt.Println(runeArr) //[97 104 109 97 100 114 101 122 97]
```

#### counting runes

```go 

func DoRuneConting() {
	a := "احمدرضا"
	fmt.Println(utf8.RuneCountInString(a))
}
```

```ad-danger
title: len 
```go 

	a := "احمدرضا"
	fmt.Println(len(a)) // 14
	fmt.Println(utf8.RuneCountInString(a)) //7
```

#### inaccurate string iteration
__printing s[i] doesn't  print the `ith` rune, it prints the `UTF-8` representation of the byte at index `i`.

```go
	a := "احمدرضاasds"
	for i, r := range a {
		fmt.Printf("i = %d, a[i]= %c, r = %c\n", i, a[i], r)
	}
	/*
	i = 0, a[i]= Ø, r = ا
	i = 2, a[i]= Ø, r = ح
	i = 4, a[i]= Ù, r = م
	i = 6, a[i]= Ø, r = د
	i = 8, a[i]= Ø, r = ر
	i = 10, a[i]= Ø, r = ض
	i = 12, a[i]= Ø, r = ا
	i = 14, a[i]= a, r = a
	i = 15, a[i]= s, r = s
	i = 16, a[i]= d, r = d
	i = 17, a[i]= s, r = s
	*/
```


_____
##### References
1.

