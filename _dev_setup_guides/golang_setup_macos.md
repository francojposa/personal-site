---
layout: resource
title: Golang - MacOS Dev Setup
slug: golang_setup_macos
---

## 1. Installation

`% brew install go`

Add the following to your ~/.zshrc:
```
export GOPATH=$HOME/go  # This is the default, but prefer explicit over implicit

export PATH=$PATH:$GOPATH/bin  # Make your binary executables available anywhere on the machine 
```
## 2. Create your Workspace
This does not have to be in your home folder, just make sure it matches what you put in GOPATH
```
% mkdir ~/go
% mkdir ~/go/bin
% mkdir ~/go/pkg
% mkdir ~/go/src
```

## 3. Your First Program
```
% mkdir ~/go/src/hello
% cd ~/go/src/hello
```
* Create & open `hello.go` in your preferred editor
* paste the following in and save:

```
package main

import "fmt"

func main() {
    fmt.Printf("Hello, World\n")
}
```
* Run it: `% go run hello.go`
* Build it (creates an executable in the same directory): `% go build hello.go`
* Run the binary: `% ./hello`
* Install the binary so it's available anywhere on your system: `% go install hello`
* Change directory to  somewhere else: `% cd ~/Downloads`
* Verify your binary is runnable from everywhere: `% hello`

### Troubleshooting
If you get `zsh: command not found: hello` when trying to run your installed binary, there is likely an issue with your path. `go install` places the binary in `$GOPATH/bin` so be sure that `$GOPATH/bin` is in your `PATH` as described in the Installation step.