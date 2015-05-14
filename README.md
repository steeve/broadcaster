# broadcaster
This package implements broadcast type messaging (or event) in a Go idiomatic
way not based on a list of handlers.

This is based on the design invented by [Roger Peppe](https://rogpeppe.wordpress.com/2009/12/01/concurrent-idioms-1-broadcasting-values-in-go-with-linked-channels/)
and the actual implementation from the [gibb package](https://github.com/dagoof/gibb),
modified to only expose channels, which are the prefered way of communicating in Go.

Contrary to ordinary packages which use a list of channels and register/unregister methods,
this package makes use of a linked list of channels to broadcast values in a
fanout way.

## Install
```
$ go get github.com/steeve/broadcaster
```

## How to use
```go
package main

import (
    "fmt"

    "github.com/steeve/broadcaster"
)

func main() {
    b := broadcaster.NewBroadcaster()
    defer b.Close()

    for i := 0; i < 10; i++ {
        go func() {
            read, done := b.Listen()
            defer close(done)

            fmt.Println(<-read)
        }()
    }

    b.Broadcast("sample payload")
}
```
