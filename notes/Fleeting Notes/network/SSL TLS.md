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
3. **Authentication**
	1. The client verifies the server's SSL certificate with the certificate authority that issued it.
	2. This confirms that the server is who it says it is and that the client is interacting with the actual owner of the domain
4. **The premaster secret**
	1. The client sends one more random string of bytes, the "premaster secret"
	2. The premaster secret is encrypted with the public key and can only be decrypted with the private key by the server.
	3. the client gers the public key from the server's SSL ceritificate
5. **private key used**
	1. The server decrypts the premaster secret
6. **Session key created**
	1. Both client and server generate session keys from client random, the server random and premaster secret. They should arrive at the same result.
7. **Client is ready**
	1. the client sends a 'finished' message that is encrypted with a session key.
8. **Server is ready**
	1. The server sends a 'finished' message encrypted with a session key
9. **Secure symmetric encryption achieved**
	1. The handshake is competed, and communication continues using the session keys.


All TLS handshakes make use of asymmetric cryptography ( the public and private key)
but not all will use the private key in the process of generating session keys.

For instance, an ephemeral Diffie-Hellman handshake proceeds as follows:


1. Client hello
	1. The client sends a client hello message with protocol version, client random and a list of cipher suites
2. Server hello
	1. The server replies with it's SSL certificate, its selected cipher suite, and the random server.
	2. in contast to RSA handshke described abote, in this message the server also include the following
3. Server's digital signature:
	1. The server computes a digital signature of all message up to this point.
4. Digital signature confirmed
	1. The client verifies the server's digital signature, confirming that the server is who its says it is
5. Client DH parameter:
	1. the client sends its DH parameter to the server
6. Client and server calculate the premaster secret
	1. instead of the client generating the premaster secret and sending it to the server, as in an RSA handshake, the client and server use the DH parametes they exchange to calculate a matching premaster secret seprately
	2. 
_____
##### References
1. https://www.youtube.com/watch?v=j9QmMEWmcfo
2. https://www.youtube.com/watch?v=yPdJVvSyMqk
3. https://www.youtube.com/watch?v=cuR05y_2Gxc


