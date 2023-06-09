+++
title = "Getting Started"
description = "Quick start with Gorgonia"
date = 2019-10-29T17:42:44+01:00
weight = -10
chapter = true
+++

## Getting gorgonia

Gorgonia is go-gettable and supports go modules.
To get the library and its dependencies, simply run

```bash
$ go get gorgonia.org/gorgonia
```

## First code to do a simple computation

create a simple program to see if the plumbing is ok:

```go
package main

import (
        "fmt"
        "log"

        "gorgonia.org/gorgonia"
)

func main() {
        g := gorgonia.NewGraph()

        var x, y, z *gorgonia.Node
        var err error

        // define the expression
        x = gorgonia.NewScalar(g, gorgonia.Float64, gorgonia.WithName("x"))
        y = gorgonia.NewScalar(g, gorgonia.Float64, gorgonia.WithName("y"))
        if z, err = gorgonia.Add(x, y); err != nil {
                log.Fatal(err)
        }

        // create a VM to run the program on
        machine := gorgonia.NewTapeMachine(g)
        defer machine.Close()

        // set initial values then run
        gorgonia.Let(x, 2.0)
        gorgonia.Let(y, 2.5)
        if err = machine.RunAll(); err != nil {
                log.Fatal(err)
        }

        fmt.Printf("%v", z.Value())
}
```

running the program should print the result: `4.5`

If instead you see the following error message:

```
panic: Something in this program imports go4.org/unsafe/assume-no-moving-gc to declare that it assumes a non-moving garbage collector, but your version of go4.org/unsafe/assume-no-moving-gc hasn't been updated to assert that it's safe against the go1.19 runtime. If you want to risk it, run with environment variable ASSUME_NO_MOVING_GC_UNSAFE_RISK_IT_WITH=go1.19 set. Notably, if go1.19 adds a moving garbage collector, this program is unsafe to use.
```

then do 

`export ASSUME_NO_MOVING_GC_UNSAFE_RISK_IT_WITH=go1.19 ` 

and then run the program again.

For further explanation, please see the [Hello World tutorial](/tutorials/hello-world).

