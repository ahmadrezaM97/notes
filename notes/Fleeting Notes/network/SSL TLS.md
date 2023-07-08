# SSL TLS
Created: 2023-07-08 19:06
Tags: 
____

![[tls-1.png]]


* TLS is an encryption and authentication protocol designed to secure internet communications.

### TLS vs SSL

* SSL or secure sockets layer, was the original security protocol developed for https
* SSL was replaced by TSL( Transport Layer security, some time ago.
* SSL handshakes are now called TLS handshakes, although the "SSL" name is still in wide use.



### What happens during a TLS handshake

1. Specify which version of TLS(TLS 1.0,1.2,1.3) they will use
2. Decide on which cipher suites they will use
3. Authenticate the identity of the server via the server's public key and SSL certificate authority's digital signature.
4. Generate session keys in order to use symmetric encruption after the handshake is complete.


### What are the stpes of a TLS handshake

1. **The 'client hello' message**
	1. The client initiates handshakes by sending a "hello" message to the server.
	2. This message include 
		1. which TLS version the client supports
		2. The cipher suites supported
		3. and a string of random bytes known as the "client random"
2. **The 'server hello' message** 
	1. in reply to the client hello message, the server sends a message containing 
		1. the server's [SSL certificate]
		2. the server's chosen cipher suite
		3. server random, another random string of bytes that's generate by the server
3. 

_____
##### References
1.https://www.youtube.com/watch?v=j9QmMEWmcfo


