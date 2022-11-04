# HTTP 1.1
Created: 2022-03-30 02:20
Tags: #http #keep-alive #upgrade-header
____
> the standardized protocol


- INDEX
	- Summery 
		- features
		- example
		- [[sss]]
	- [[Keep-Alive]]
	- [[HTTP Upgrade Header]]


#### Summery
- This is the HTTP version currently in common use.
- Introduced critical performance optimizations
- Method supported:
	- GET (v0.9) |  HEAD, POST (v1.0) |  PUT, DELETE, TRACE, OPTION (new supported)
- connection nature: __LONG-LIVED__

![[httpv1-vs-v1.1.gif]]

```ad-example 
title: connection example
collapse: true

**(Connection 1 Establishment - TCP Three-Way Handshake)**  
Connected to xxx.xxx.xxx.xxx**(Request 1)**  
GET /en-US/docs/Glossary/Simple_header HTTP/1.1  
Host: developer.mozilla.org  
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate, br  
Referer: https://developer.mozilla.org/en-US/docs/Glossary/Simple_header  
  
**(Response 1)**  
HTTP/1.1 200 OK  
Connection: Keep-Alive  
Content-Encoding: gzip  
Content-Type: text/html; charset=utf-8  
Date: Wed, 20 Jul 2016 10:55:30 GMT  
Etag: "547fa7e369ef56031dd3bff2ace9fc0832eb251a"  
Keep-Alive: timeout=5, max=1000  
Last-Modified: Tue, 19 Jul 2016 00:59:33 GMT  
Server: Apache  
Transfer-Encoding: chunked  
Vary: Cookie, Accept-Encoding  
  
[content]  
  
**(Request 2)**  
GET /static/img/header-background.png HTTP/1.1  
Host: developer.cdn.mozilla.net  
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0  
Accept: */*  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate, br  
Referer: https://developer.mozilla.org/en-US/docs/Glossary/Simple_header  
  
**(Response 2)**  
HTTP/1.1 200 OK  
Age: 9578461  
Cache-Control: public, max-age=315360000  
Connection: keep-alive  
Content-Length: 3077  
Content-Type: image/png  
Date: Thu, 31 Mar 2016 13:34:46 GMT  
Last-Modified: Wed, 21 Oct 2015 18:27:50 GMT  
Server: Apache  
  
[image content of 3077 bytes]**(Connection 1 Closed - TCP Teardown)**

```



New words
	- obsoleted
	- 
_____
##### Refrences
1.[Evolution of http](https://medium.com/platform-engineer/evolution-of-http-69cfe6531ba0)



