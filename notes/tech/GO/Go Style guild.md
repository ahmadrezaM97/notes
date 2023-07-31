
Created: 2022-11-23 21:59
Tags: 
____

### Style Principles

* __Clarity
	* The code's purpose and rationale is _clear_ to the reader.
* __Simplicity___
	* The code accomplishes it's goal in the simplest way possible.
* __Maintainability__
	* The code is written such that it can be easily maintained.
* __Consistency__
	* The code is consistent whit the broader codebases


### Clarity

The __Core goal__ of readability is to produce code that is __clear__ to the reader.

* Clarity is primarily achieved with
	* effective naming
	* helpful commentary
	* efficient code organization

Clarity has two distinct facets
* __What is the actually doing? ( the what )
	*  In cases of uncertainty or where a reader may require prior knowledge in order to understand the code -> it worth investing time in order to make the code's purpose clearer for future readers
		* User more descriptive variable names
		* Add additional commentary
		* Break up the code with whitespace and comments
		* Refactor the code into separate functions/methods to make it more modular
	* The is no one-fits-all approach here, but it is important to prioritize clarity when developing go code.
* __Why is the code doing what is does? ( the why)
	* The `Why?` is especially important when the code contains nuances that a reader may not be familiar with, such as:
		* A nuance in the language
			* a closure will be capturing a loop variable the closure us many lines away
		* A nuance in the business logic
			* an access control check that needs to distinguish between the actual user and someone impersonating a user

Readers must understand code without needing to reverse-engineering it
Typically for performance, we add complexity and we need make code clear










_____
##### References
1.

