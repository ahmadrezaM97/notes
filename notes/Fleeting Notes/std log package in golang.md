
  


### Log in standard packages

- Why we should not use __fmt__ package``
- Package log is safe from concurrent goroutines while plain ___fmt___ is not.
- Log can attach useful data automatically to record


```go
	log.Println("something")
```


##### we can use flag 
-  log.Ldata -> date
-  log.Lshortfile -> filename

```go
	log.SetFlags(log.Ldate | log.Lshortfile)
	log.Println("something else")
```


##### Log in file

```go
file, _ := os.Create("file.log")

log.SetOutput(file)

log.Println("this would create in file (file.log)")

// or
log.SetOutput(os.Stdout)
```

##### Log levels 

``` go
flags := log.Llongfile | log.Lshortfile

infoLogger := log.New(os.Stdout, "INFO: ", flags)
warnLogger := log.New(os.Stdout, "WARN: ", flags)
errorLogger := log.New(os.Stdout, "ERROR: ", flags)

  

infoLogger.Println("this is an info log")
warnLogger.Println("this is a warn log")
errorLogger.Println("this is a error log")

```


#### Refrences
[__Youtube Video__](https://www.youtube.com/watch?v=yF7k6PxtRU8)


#### Links
[[Logging]]
