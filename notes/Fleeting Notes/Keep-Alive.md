# Keep-Alive
Created: 2022-03-30 02:47
Tags: #http/1.1 #headers
____

#### Keep-Alive and Upgrade headers
- The Keep-Alive header was used prior to HTTP/1.1 and was obsoleted by HTTP/1.1 making persistent connections the default behavior. [?]
- Client, server or any intermediary can provide information for Keep-Alive header independently.
- __A host can add timeout and max parameters in order to set a timeout or limit maximum request count per connection.__
-
```ad-example 
title: connection example
collapse: true
HTTP/1.1 200 OK  
**_Connection: Keep-Alive_**  
Content-Encoding: gzip  
Content-Type: text/html; charset=utf-8  
Date: Thu, 11 Aug 2016 15:23:13 GMT  
**_Keep-Alive: timeout=5, max=1000_**  
Last-Modified: Mon, 25 Jul 2016 04:32:39 GMT  
Server: Apache  
  
[body]

```

![[keep-alive-eg.png]]

```ad-quote

HTTP pipelining, multiple connections, and many more improvements have been implemented, thanks to the `Keep-Alive` header’s behavior.

```


_____
##### Refrences
1.[Evolution of http](https://medium.com/platform-engineer/evolution-of-http-69cfe6531ba0)


