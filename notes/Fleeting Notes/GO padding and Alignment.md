# GO padding and Alignment
Created: 2022-10-29 13:02
Tags: 
____

* How mich memory is allocated for a value of type example?
``` go

type example struct {
	flag bool // 1byte
	counter int16 // 2 byte
	pi float32 // 4 byte
}
```


1bytes + 2bytes + 4 bytes is really 7byte? no no no no

The right answer is 8 bytes
![[go-padding.png]]
* because there is padding byte sitting between the flag and counter fields for the reason of alignment
* The idea of alignment is to allow the hardware to read memory more efficiently by placing memory on specific alignment boundaries.
* The compiler take care of the alignment boundary mechanics so I don't have to.



_____
##### References
1.

