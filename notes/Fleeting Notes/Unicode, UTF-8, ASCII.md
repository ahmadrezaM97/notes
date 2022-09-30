# Unicode, UTF-8, ASCII
Created: 2022-09-30 22:36
Tags: 
____


### ASCII
* First the re was the C programming language, then there ware ASCII.
* __In ASCII every letter, digits, and symbols that matters (a-z,A-Z,+,-,/,~ etc)
were represented as number between 21 and 127.

### Unicode
* The Unicode was a brave attempt to crate a single character set that represent every characters in every imaginable language system.
* This required a paradigm shift in how to interpret characters.
each character -> abstract entity
* __rather then use a number, each character was represented as a code-point.
* the code-point were written as U+00639, where U stand for `Unicode` and the number are hexadecimal.

### UTF-8  ( Unicode  Transformation format - 8- bit)
* the final piece we're missing at this point is a system for storing and representing these `code-point`
* This is what the encoding standards serve.
* After a couple of hits and misses, the `UTF-8` encoding standard was born.

```ad-tip
title: UTF-8

In UTF-8, every code-point from --127 is stored in a single byte.

Code points above 128 are stored using 2,3 and in fact, up to 6 bytes

```

* Other benefits of UTF-8 meant that nothing changed from the ASCII so far as the basic english character-set was considered.

__These is NO string or text, without an accompanying encoding standard.



* Charset
	* A charset, as the name suggests, is a set of characters
		* for instance: the Unicode charset contains 2^21 characters.
* Encoding
	* An encoding is the translation of a character's list in binary.
		* `UTF-8` is an encoding standard capable of encoding all the Unicode characters in a variable number of bytes ( from 1 to 4 bytes)
_____
##### References
1.

