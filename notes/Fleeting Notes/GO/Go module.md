# Go module
Created: 2023-01-07 02:33
Tags: 
____

* Go module support is intended to solve several problems
	* avoid the need for `$GOPATH`
	* group packages versioned/released together
	* support semantic versioning & backward compatibility
	* provide in-project dependency management
	* __Offer strong dependency security & availability__
	* continue support of `vendoring`
	* __work transparently across the Go ecosystem.

```ad-tip
title: GO modules proxying
go modules proxying offers the value of vendoring without requiring your prokect to vendor all the 3rd-party code in your repo.
```

Go's dependency management protects against some risks:
	1. flaky repos
	2. package that disappear
	3. conflicting dependency versions
	4. surreptitious changes to public packages

BUT it cannot ensure the actual quality of security of the original code

"A little copying is better than a little dependency" go provebs

#### import compatibility rule

__If an old packages and a new package have the same import path, the new package must be backward compatible with the old package__

```go
package hello

import (
	"github.com/x"
	x2 "github.com/x/v2"
)
```

Note that you can import both versions if necessary


### `go.mod`

the `go.mod` file has your module name along with direct dependency requirements (and since go 1.13, the version of go)


```
module hello
require github.com/x v1.1
go 1.13
```

some important environment variables

```bash
GOPROXY=https://proxy.golang.org,direct
GOSUMDB=sum.golang.org
```

ans set this for private  repositories

```bash
GOPRIVATE=github.com/xxx, github.com/yyy
GONOSUMDB=githiub.com/xxx, github.com/yyy
```

remember also you must be set up for access to private `Github` repos in order to download private modules

![[go-module-proxy.png]]



```ad-warning
title: pseudo-versions
go.mod file my recored pseudo-versions for non-release/trunk versions as well as "replacements"
```


#### Useful commands

```bash
go mod init <module-name> ## create the go.mod file
go get -u ./... ## update transitively
go mod tidy ## remove uneedned modules
go list -m -versions <module_path> ## list available versions of a dependency
go get <module-path> ## add and install a dependency

```

```ad-note
Go keeps alocal cache in `$GOPATH/pkg`
	. each package (using a directory tree)
	. the hash of the root checksum DB tree


__Use `go clean -modecahe` to remove it all (i.e., in make clean)__
```


### Daily Workflow

day-to-day workflow can


_____
##### References
1.

