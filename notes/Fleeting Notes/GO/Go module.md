# Go module
Created: 2023-01-07 02:33
Tags: 
____

### definition

A module is a collection of Go packages stored in a file tree with a `go.mod` file at its root.

#### `go.mod` file

A module is defined by a `UTF-8` encoded text file named `go.mod` in its root directory.

the `go.mod` file is line-oriented. each line holds a single directive, made up of a keyword followed by arguments.

```
module example.com/my/thing

go 1.12

require example.com/other/thing v1.0.2
require example.com/new/thing/v2 v2.3.4
exclude example.com/old/thing v1.2.3
replace example.com/bad/thing v1.4.5 => example.com/good/thing v1.4.5
retract [v1.9.0, v1.9.5]

```


the leading keyword can be factored out of adjacent lines to create a block

```
require (
    example.com/new/thing/v2 v2.3.4
    example.com/old/thing v1.2.3
)
```


* the `go.mod` file is designed to be human readable and machine writable.
* the go command provides several subcommands that change `go.mod` files

```ad-example
title: for example 
`go get` 
can upgrade or downgrade specific dependencies.
```

* commands that load the module graph will automatically update `go.mod` when needed.
* `go mod eidt` can perform low-level edits
* `golang.org/mod/modfile` package can be used by Go programmers to make the same changes automatically


### directives

#### `module` 
a `module` directive defines the main module's path
__A `go.mod` file must contain exactly one module directive__

```
ModuleDirective = "module" ( ModulePath | "(" newline ModulePath newline ")" ) newline .

```


##### `require` directive

A `require` directive declares a minimum required version of a given module dependency.
for each required module version, the go com

#### `go`
#### `Deprecation`



The `go.mod` file defines the module's `module path` which is also the import path used for the root directory, and its `dependency requirements` which are the other modules needed for a successful build.
each dependency requirement is written as a module path a specific [[semantic version]].


```ad-note

Each package within a module is a collection of source files in the same directory that are complied together
```

A package path is the module path joined with subdirectory container the package ( relative to the module root)

module `golang.org/x/net` contains a package in directory "html"
the package's path is `golang.org/x/net/html`



_____
##### References
1.

