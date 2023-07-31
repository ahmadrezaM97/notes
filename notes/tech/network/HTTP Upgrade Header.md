# HTTP Upgrade Header
Created: 2022-03-30 02:49
Tags: #http #http/1.1 #http/2.0 #websocket #http_header
____


- With Upgrade header introduced in HTTP/1.1, it is possible to start a connection using a commonly-used protocol. such as HTTP/1.1 
Then request that that the connection switch to an enhanced protocol type like [[HTTP/2.0]] or [[WebSocket]].
- In an upgraded protocol connection, __max__ parameter(max request count per connection) is NOT present.
	- The upgraded protocol can provide new policies for ___timeout__ parameter
		- if not specifically defined, it uses default timeout value in underlying protocol

![[http-upgrade-header.png]]
_____
##### Refrences
1.[Evolution of http](https://medium.com/platform-engineer/evolution-of-http-69cfe6531ba0)



