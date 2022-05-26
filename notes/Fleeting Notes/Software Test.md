# Software Test
Created: 2022-05-01 14:59
Tags: 
____

# Unit test
is an integral part of the [[SDLC]]
type of software testing where individual units or components of a software are tested

> not code or code review should be accepted without accompanying unit tests.



```ad-tip

there is no defined scope for a "Unit"

for instance, in oop a whole class or interface is treated as a unit
but in procedural programming, an individual function might be treated as a unit

```



![[characteristics-of-a-good-unit-test.png]]

#### Fast
#### Reliable
Unit tests only fail if there is a bug in the underlying code
#### Isolated
Unit tests are run in isolation without any internal dependencies and external dependencies(database,file system, etc.) or external environment factors.
#### Self-checking
Automatic
#### Timely
easy to write and not tightly coupled

#### Truly unit, not integration
Unit tests can easily turn into integration tests if tested together with multiple other component. Unit tests ate standalone and should not 



avoid magic nubmer
 ### Refrain multiple asserts in a single unit test
 ### Automate tests using CI/CD tools
### How does Unit Testing Works
![[automate-unit-tests-in-cicd--1536x827.png]]

[[Test coverage]]
## Should be:
- __Test should be fast__
	- If they are slow, developers wont' run them as often as they should
	- how do make your test fast:
		- simple as possible
		- don't make them depend on other tests
		- mock external dependencies
- __Should be Simple__
- __Should be Readable___
- __Should be deterministic__
- __Should be a part of build process__
	- in CI for instance 
- __Unit Test cases should be independent__
- __Test only one code at a time.__
- __Follow clear and consistent naming conventions for your unit test
- __Before next stage you must pass all unit test
- 

### Advantage
- due to the modular nature of unit test, we can do partial testing
- whit good unit test developers could use it as a simple doctumtion of how to use a component in our software
- Detect code smells int your code base
	- If it is hard to write unit test it could a sign of code smell



#### unit test vs inegration test
It is all about the level of isolation.

- Integration Test
- Regression Test
- Smoke Test
- Alpha Test
- Beta Test
- System Test
- Performance Test



What is the benifts of testing?

How you often test your sotware?

What is a good test?

What is TDD

https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices

https://www.simform.com/blog/functional-testing-types/
https://www.simform.com/blog/what-is-integration-testing/
_____
##### References
1.

