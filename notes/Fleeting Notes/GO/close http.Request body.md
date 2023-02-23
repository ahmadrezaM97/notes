# close http.Request body
Created: 2023-02-23 18:49
Tags: 
____

According to the documentation of `Golang`, if the `Body` inside response is not closed and read to `EOF`, the client my not re-use a persistent `TCP` connection to the server.


_____
##### References
1.

