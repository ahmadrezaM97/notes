# SOLID
Created: 2023-05-01 17:05
Tags: 
____

1. S: Single responsibility
2. O: Open/Close principle
3. L: Liskov substitution principle
4. I: Interface segregation principle
5. D: Dependency inversion principle

Bad code
1. Rigidity
	1. code to be difficult to change even if change is small
2. Fragility
	1. Code to break whenever a new change is introduced in the system
3. Immobility
	1. Code being not reuseable
4. Viscosity


### Single Responsibility Principle

```ad-quote
title: Unix philosopy
Do one thing and do it well
```


* Historically, the SRP has been described this way
	* A module should have one, and only one, reason to change.
* Software are changed to satisfy users and stakeholders
* those `users` and `stakeholders` are REASON TO CHANGE.
	* a module should be responsible to one, and only one, user or stakeholder.
* Unfortunately, user and stakeholders aren't the best words here
* we are referring to a group who require that change, we'll refer to that group as an `actor`

```ad-quote
a module should be responsible to one, and only one, actor.
```

Cohesion is the force that bind together the code responsible to a single actor



* two separate aspects of a problem need to be handled by different module.
	* changes in a module should be originated from only on reason.
* A Class should have only one primary responsibility and should not take other responsibility.
	* A class should have only one reason to change




#### Example

```python
import json

class TelephoneDirectory:
    def __init__(self):
        self.data:dict[int,str]  = {}
    
    def add_entry(self, name:str, number:int) -> None:
        self.data[name] = number 
    
    def delete_entry(self, name:str) -> None:
        self.data.pop(name)
    
    def look_number(self, name:str) -> str:
        return self.data.get(name)

    def save_to_file(self, path:str)->None:
        with open(path, "w") as file:
            json.dump(self.data, file)
```


```python
import json

class TelephoneDirectory:
    def __init__(self):
        self.data:dict[int,str]  = {}
    
    def add_entry(self, name:str, number:int) -> None:
        self.data[name] = number 
    
    def delete_entry(self, name:str) -> None:
        self.data.pop(name)
    
    def look_number(self, name:str) -> str:
        return self.data.get(name)

class DictionaryPersis:
    def persists(self,dictionary: TelephoneDirectory ,path):
        with open(path, "w") as file:
            json.dump(dictionary.data, file)
```




```go
```





### Open/Closed Principle

```ad-quote
titile: Robert C.Martin
A module should be open for extensions, but closed for modification.
```

* modules should be written in a way so that we can add new modules or new functionalities in a module without requiring existing modules to be modified
* 


_____
##### References
1.

