shy # Unicode
Created: 2022-10-13 10:51
Tags: 
____
* To store data we use binary, for numbers it's easy to use equivalent number in base 2, but what about characters
* The solution is a 
	* collectively agreed mapping

### ASCII
* American standard code for information interchange
* is a character encoding standard

![[Ascii_table.png]]


BUT WHAT ABOUT OTHER LANGUAGES? OR OTHER SYMBOLS?

__Unicode__ Universal Coded Character set
* Unicode takes the role of providing a unique code point __ a number __ for each character.
* The Unicode Standard defines a code space, a set of numerical values ranging from `0` through `10FFFF` called `code points`
* and denotes as `U+0000`  through `U+10FFFF`

we replace `charachter` with `Grapheme`
-> one or more code points

Next Step is converting code point to binary (Encoding)

### `UTF-8`
* `UTF-8` as a variable-width character encoding.
* `UTF-8` is capable of encoding all 1,112,064 valid character `code point` in `Unicode`  using one to four byte(8-bit)
* Code points with lower numerical  values, which tend to occur more frequently , are encoded using fewer bytes.
	* It was design for backward compatibility with ASCII first 128 characters of `Unicode`, which correspond one-to-one with `ASCII` are encoded using a single byte with the same binary value as `ASCII`


### InPython
* Since python3.0 the language's `str` type contains `Unicode` characters
	* -> any string created using `"unicode rocks!"` `unicode talks!`, or the triple-quotes string syntax is stored as `Unicode`
* Default encoding python source code is `UTF-8`, so you can simply include a `Unicode` character in a string literal
* python 3 also supports using `Unicode` characters in udentifiers
```python
سلام = "salam"
print(سلام)
```

bytes to Unicode 

byte.encode
string.decode

```python
a = b'\xd8\xb3\xd9\x84\xd8\xa7\xd9\x85'.decode("utf-8")
print(a) #سلام

try:
	b = b'\x80abc'.decode("utf-8")
except UnicodeDecodeError as e:
	print("error")
# UnicodeDecodeError: 'utf-8' codec can't decode byte 0x80 in position 0: invalid start byte

print(b'\x80abc'.decode("utf-8","ignore"))
# 'abc'

 b"\x62".decode('utf-8')  # 'b'
 b"\x62".decode('ascii')  # 'b'

"a\xac\u1234\u20ac\U00008000"  # 'a¬ሴ€耀'
```

```python

# code point -> char (default utf-8)
print(chr(97)) # a
print(chr(297)) # 'ĩ'
print(chr(31999)) # 糿

# unicode -> code point ( default utf-8)
print(ord('\ue020'))  #57376
```




___



__
##### References
1.

