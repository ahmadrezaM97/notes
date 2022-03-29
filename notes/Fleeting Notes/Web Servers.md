# Web Servers
Created: 2022-03-30 00:21
Tags: #webservers #http #http 
#todo
____

##### What is a web server

1. Software that servers __web content__
	1. html, css,  json docs and ...
2. Uses that [[HTTP protocol]].
3. Static and Dynamic content.
4. Used to host web pages, blogs and build APIs.


##### How Web Servers Work?
- Usually use 80 for http, and 480 for [[Https]]

#### Blocking Single-Threaded Web Server
- Every client reserve a little bit of memory for that connection( [[TCP protocol]] socket )
- If you have single process, single threaded  while it's processing a request it can not do anything else.
- when new client try to connect webserver just established a new connection with that and do nothing until it's done.



???
OTS Web servers :  httpd(Apache)  IIS  lighttpd  tomcat  http-server
Write your own! : nodejs  python tornado
???


#### TODO
 - [ ] try simple apache, webserver , and try to see what happens when use have single thread single process blocking web server
 - [ ] try sample nodejs or python webserver
	
_____
##### Refrences
1.[ Nasser Youtube](https://www.youtube.com/watch?v=JhpUch6lWMw)

