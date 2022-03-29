# HTTP 0.9
Created: 2022-03-30 04:50
Tags: #http 
____

- Initial version of HTTP 
	- Simple client-server, request-response protocol
	- Telnet friendly protocol
		- We know  [[HTTP 1.1]]  was browser friendly
- Request nature: single-line 
	- method + path for requested document
- Methods Supported: 
	- GET only
- Response type 
	- hypertext only
- connection nature:
	-  terminated immediately after the response. (like HTTP 1.0)

```ad-info
title: NO

No HTTP HEADERS
cannot transfer other content type files
Not status/error codes
No URLs
No Versioning

```

```ad-example
title: connection example
collapse: true

$> telnet ashenlive.com 80**(Connection 1 Establishment - TCP Three-Way Handshake)  
**Connected to xxx.xxx.xxx.xxx**(Request)**  
GET /my-page.html**(Response in hypertext)**  
<HTML>  
A very simple HTML page  
</HTML>**(Connection 1 Closed - TCP Teardown)**

```

*poplular webserver (Apache, Nginx) still supports HTTP0.9*

_____
##### Refrences
1.[Evolution of http](https://medium.com/platform-engineer/evolution-of-http-69cfe6531ba0)



