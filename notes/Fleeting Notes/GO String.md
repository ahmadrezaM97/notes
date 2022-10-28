# GO String
Created: 2022-10-08 10:00
Tags: 
____

#### What is a string?
* Types related to strings:
	* `byte`: a synonym for `uint8`
	* `rune`: synonym fr `int32` for characters
* an __immutable__ sequence of "characters"
	* physically a sequence of bytes(`UTF-8` encoding)
	* logically a sequence of (`unicode`) runes
* In Go, a string is in effect a __read-only__ slice of bytes.
* It's important to state right up front that a string hold `arbitrary` bytes.
	* It is not required to hold Unicode text, `UTF-8` text, or any other predefined format
	* As far as the content of a string is concerned, it is exactly equivalent to a slice of bytes.
	* Arbitrary bytes can also be included in literal string using hexadecimal or octal escapes
		* hexadecimal escape is written `\xhh`
		* octal escape is written `\ooo`

> strings are just sequence of arbitrary bytes

Because some of the bytes in out sample string are not valid ASCII, not even valid `UTF-8`, print the string will produce ugly output.
```go
	const sample = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"
	fmt.Println(sample)
	// ��=� ⌘
```

#### Rune

* A `rune` is an alias to the `int32` data type.
* `rune` It represent a Unicode code point.
* Go source code us `UTF-8`.
* `String` can contain `Unicode` text encoded in `UTF-8`, which encodes all Unicode points using one to four bytes.
* A string is a sequence of bytes; more precisely, a slice of arbitrary bytes.

```go
type rune = int32
```

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

#### Bytes vs rune

```go 
	s := "آ a"
	fmt.Println([]rune(s)) // [1570 32 97]
	fmt.Println([]byte(s)) // [216 162 32 97]
```

#### Len of string vs number of runes ( how to count runes)
``` go
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

#### inaccurate string slice

```go
func DoStringSlice() {
	s := "احمدرضا"
	fmt.Println(s[:5]) // اح�
}
```

#### Immutability
```go
func DoChangeCharInString() {
	// strings are read-only | immutable
	s := "ahmadreza"
	s[0] = 'o' // compile error: cannot assign to s[0]
}
```

#### Go source file are always encoded in `UTF-8`

### string literal
* A raw string literal is written \`...\` using backquotes instead of double quotes
* Within a raw string literal, no escape sequences are processed
* the contents are taken literally, including backslashes and newlines
* a raw literal my spread over several lines in the program source
* Raw string literal are a convenient way to write regular expressions
	* which tends to have lots of backslashes
* They also useful for HTML templates, JSON literals, command usage messages 
```go
func DoStringLiteral() {
	a := `
		\sd \sds \a sdlaksjd 
		asd\ asd\r
	`
	fmt.Println(a)
}

```


#### Under-optimized string concatenation


Using `string.Build`, we can also append
1. A byte slice using Write
2. A single byte using `WriteByte`
3. A single rune using `WriteRune`


```go
func DoStringConcatenation(){
	values := []string{"a", "b"}
	total := 0
	for i := 0; i < len(values); i++ {
		total += len(values[i])
	}
	sb := strings.Builder{}
	sb.Grow(total)
	for _, value := range values {
		_, _ = sb.WriteString(value)
	}
	fmt.Println(sb.String())
	
}
```


 #### `UTF-8`
* *`UTF-8` is a variable-length encoding of Unicode code points as bytes
* UTF-8 was invented by Ken Thompson and Rob Pike, two of the creators of GO, and is now a Unicode standard.
* It uses between 1 and 4 bytes to represent each rune, but only 1 byte for ASCII characters and only 2 or 3 bytes for most runes in common use.
* Go source file is always UTF-8

_____
##### References
1.

