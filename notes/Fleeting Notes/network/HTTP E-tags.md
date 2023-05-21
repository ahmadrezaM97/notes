# HTTP E-tags
Created: 2022-03-30 01:20
Tags: #Etag #http #caching #todo #Bandwidth #load_balancer
____


#### Summary

The ETag (or entity tag ) HTTP response header is an identifier for a specific version of a resource.

It's lets change be more efficient and save __bandwidth__, as a web server does not need to resend full response if the content was not changed.

If the resource at a given URL changes, a new Etag value must be generated.

![[e-tag-1.png]]
![[e-tag-0.png]]



* some of web servers by default handle E-tags

Pros:
- Fast Response
- Save Bandwidth
-  consistency
	-  [[mid-air collisions]]

Cons:
* Load-Balancer
	* Problem 
		* when we use load balancer every server could make own e-tag
	* Solution
		* config! :)))
* If you right your client you must implement this mechanism in client-slide
* It's could use for tracing client 
	
_____
##### Refrences
1. [mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#avoiding_mid-air_collisions)
2. [Nasser Youtube](https://www.youtube.com/watch?v=TgZnpp5wJWU)

