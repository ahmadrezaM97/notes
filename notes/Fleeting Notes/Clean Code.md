# Clean Code
Created: 2022-07-09 04:25
Tags: 
____


##### Mutable Default Arguments

```python
def addElements(a=[]):
	a.append(5)
	return a

addElements()
# [5]
addElements()
# [5, 5]
```

##### Using Build-in Function

``` python
### ZIP

### product
li  = []
l1=[1, 2, 3]
l2=[4, 5, 6]
for x,y in itertools.product(l1,l2):
	li.append((x,y))	


### overdoing list comprehension when it wasn't needed

x = ["A", "B", "C", "D"]
# Bad
[x.lower() for x in names]

# Good
list(map(str.lower, names))

```

##### DataClasses

```python

from dataclasses

@dataclasses:
class Country:
	name:str
	population: int
	are
```