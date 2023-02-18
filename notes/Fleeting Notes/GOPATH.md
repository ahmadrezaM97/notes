# GOPATH
Created: 2023-02-18 11:47
Tags: 
____

GOPATH
GOPATH, also called the `worksapce` direcotory, is the directory where the Go code belongs, IT is implemented by and documented in the `go/build` package and is used to resolve import statements.

The `go get` tool downloads packages to the first directory in `GOPATH`

If the environment variable is unset, `GOPATH` defaults to a subdirectory named "go" in the user's home directory.


1. `src`
	1. it holds source code, the path below this directory determines the import path or the executable name
2. `pkg`
	1. it holds installed packages objects, Each target operating system and architecture pair has its own subdirectory of pkg
3. `bin`
	1. it holds complied commands. Every command ios named for its source driectory

https://www.geeksforgeeks.org/golang-gopath-and-goroot/

```ad-note

Whe using modules in GO, the GOPATH is no longer used to determine imports. However, it is still used to store downloaded source code in pkg and compiled commands `bin`
```


4. Go programmers typically keep all their Go code in a single workspace
5. A workspace contains many version control repositories(managed by Git for example)
6. Each repository contains one or more packages
7. Each package consists of one or more Go source files in a single directory
8. The path to a package's directory determines its import path

Workspace
A workspace is a directory hierarchy with two directories at its root
1. src
2. bin

https://go.dev/doc/gopath_code
_____
##### References
1.

