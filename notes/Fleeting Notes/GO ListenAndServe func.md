# GO ListenAndServe func
Created: 2022-03-31 03:42
Tags: 
____

__ListenAndServer__ listen on The [[TCP protocol]] address src.Addr and then calls Serve to handle request on incoming connections.

Accept connection are configured to enable TCP [[Keep-Alive]]

__ListenAndServe__ always return a non-nil error.After shoutdown or close,the returend error is ErrServerClosed

```go
func (srv *Server) ListenAndServe() error {
	if srv.shuttingDown() {
		return ErrServerClosed
	}
	addr := srv.Addr
	if addr == "" {
		addr = ":http"
	}
	ln, err := net.Listen("tcp", addr)
	if err != nil {
		return err
	}
	return srv.Serve(ln)
}
```
_____
##### Refrences
1.

