# HTTP 1.0
Created: 2022-03-30 01:50
Tags: #TCP #Browser
____

### Summery
- Browser-friendly protocol
- Provided header fields including right 
	- http version num.
	- status code
	- content type
- metadata about both request and response
- Response: NOT Limited to hypertext
	- content-type header provided ability to transmit files other that plan HTML files
		- scripts, stylesheet, media
* supported: GET, HEAD, POST
-  Connection nature:
	- TERMINATED IMMEDIATELY AFTER THE RESPONSE
- Each request open TCP connection, send request , get response and then close the connection.

```ad-example
title: connection example
collapse: true

(Connection 1 Establishment - TCP Three-Way Handshake)
Connected to xxx.xxx.xxx.xxx


(Request)
GET /my-page.html HTTP/1.0   
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)**


(Response)  
HTTP/1.0 200 OK   
Content-Type: text/html   
Content-Length: 137582  
Expires: Thu, 01 Dec 1997 16:00:00 GMT  
Last-Modified: Wed, 1 May 1996 12:45:26 GMT  
Server: Apache 0.84  
  
<HTML>   
A page with an image  
  <IMG SRC="/myimage.gif">  
</HTML>

(Connection 1 Closed - TCP Teardown)

```


![[tcp-handshake.png]]

### Problem
```ad-danger
title: Problem 

Establishing a new connection for each request — major problem in both HTTP/0.9 and HTTP/1.0

Both HTTP/0.9 and HTTP/1.0 required to open up a new connection for each request
and close it immediately after the response was sent

Eeach time a TCP three-way handshake should occur
```


_____
##### Refrences
1.[Evolution of http](https://medium.com/platform-engineer/evolution-of-http-69cfe6531ba0)

