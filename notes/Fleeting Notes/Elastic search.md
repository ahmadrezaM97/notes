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


BKTree for integers


### Shard

Elasticsearch provide the ability to subdivide the index into multiple pieces called shards.

Each shard is in itself a fully-functional and independent "index" that can be hosted on any node within a cluster.


### Replica

* Basically, a replica shard is a copy of a primary shard
* replica provide redundant copies of your data to protect against hardware failure and 
	* increase capacity to serve read request like searching or retireving a document.
* 

#### Document default fields

* `__Index` a logical namespace that groups one or more shards. Not stored and not indexed.
* `__Type` the class of object that the
* 



### Retrieving a Document

`GET post/_doc/AZ40lvfl`

`GET post/_doc/AZ40lvfl?pretty`

#### Retrieving Part of a Document

`GET post/_doc/AZ40lvfl?_source=data.title`

#### without metadata

`GET post/_doc/AZ40lvfl/_source`

``` bash
curl 'http://172.19.40.23:9200/post/_doc/AZ40lvfl' -H 'Authorization: Basic ZWxhc3RpYzpaeEVnOHh4X25CLXlOLWhk' 
```


_____
##### References
1.

