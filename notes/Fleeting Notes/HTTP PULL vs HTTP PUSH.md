# HTTP PULL vs HTTP PUSH
Created: 2022-05-19 19:57
Tags: 
____

### HTTP PULL
![[http-pulling.png]]

-> This is the default mode of HTTP communication, called the HTTP PULL mechanism.

the client pulls the data from the server whenever required.
It keeps doing this over and over to fetch the latest data.


#### The disadvantage
 If clients keep on periodically makes the pull request for updated data, but there are not updates at the server hence.
 Every time the client will get the same result, bandwidth will be wasted and the server will be busy too.


#### HTTP PUSH
![[http-push.png]]

To overcome the problem with HTTP pull, an HTTP push as introduced. In the HTTP push method, the client opens a connection to the server by requesting a server only the first time and after that server keeps on pushing back updated content to the client, whenever there's any.

Push services are often based on the information preferences of the users expressed in advance. This is called a publish/subscribe model.

An example of the HTTP push request is Facebook/Twitter notification.



_____
##### References
1.

