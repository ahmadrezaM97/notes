# write skew
Created: 2023-01-03 03:25
Tags: 
____
[[dirty writes]] and [[lost updates]] are two kinds of race conditions, but we also have THE WRITE SKEW.

Intuitive example

![[write-skew-example.png]]

Imagine the Alice and Bob are the two on-call doctors for a particular shift. both are feeling unwell, so they both decided to request leave.
In each transaction, your application first checks that two or more doctors are currently on call.
if yes, it assumes it's safe for one doctor to go off call.

Since that base use  snapshot isolation, both checks return 2, so both transaction commit. WTF


#### Characterizing write skew







_____
##### References
1.

