# Elastic search
Created: 2023-04-30 22:13
Tags: 
____

#### Introduction

* characteristics
	* JSON based DS
	* RESTFUL
	* LOGs,  MEtrics, App traces


* Documents -> row
* Index -> documents that have similar characteristics (database)
* Mapping
	* like the schema of sql tables
	* distinguish between full-text strings fields and exact value string fields
	* Perform language-specific text analysis
	* Optimize fields for partial matching
	* use custom data format
	* Use data types that cannot be automatlly detected

what happend when a fields is not in mapping



#### Invert index

Elastic just know how to search in index not their data
If a fields is not indexed, you can not search for that
it's for strings texts



_____
##### References
1.

