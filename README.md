> This wiki isn't completed. Link to the content table items will be added after completing each page!

Welcome to the unofficial vecty doc

[Vecty](https://github.com/gopherjs/vecty) is a [React](https://facebook.github.io/react)-like library for [GopherJS](https://github.com/gopherjs/gopherjs) so that you can do frontend development in Go instead of writing JavaScript/HTML/CSS.

**Before you dive into this, you should know some basic of Golang, Javascript, Html and some frontend knowledge.**

* Getting Started
    * [Installation](#installation)
    * [Hello world](#vecty-hello-world)
    * [Vecty components](#vecty-components)
    * Vecty elements
    * Vecty properties
    * Vecty styling
    * Vecty data attribute
    * Vecty DOM event
    * Vecty render and rerender
* Examples
    * WORK IN PROGRESS

# Installation

> The [vecty](https://github.com/gopherjs/vecty) project is experimental. You should consider it's [issues](https://github.com/gopherjs/vecty/issues/) before putting it into production!

## Install via `go get`

You can install `vecty` by the `go get` command

```sh
go get -u -v github.com/gopherjs/vecty
```

The output should be something like this:

```txt
github.com/gopherjs/vecty (download)
github.com/gopherjs/gopherjs (download)
```

Done!

Now run `go tool doc vecty` to see the documentation. You may also see the documentation at [godoc.org/github.com/gopherjs/vecty](https://github.com/gopherjs/vecty).


# Vecty Hello World

This tutorial will aim to introduce you to basic Vecty concepts.

You should create a project directory inside your `$GOPATH`. In this tutorial we will use `$GOPATH/src/vecty-example`

To begin, we 
will render a simple web page. Create a file **main.go** and paste the following
contents. 
 ```go
package main
import (
    "github.com/gopherjs/vecty"
    "github.com/gopherjs/vecty/elem"
)
 func main() {
    // Set the HTML page title
    vecty.SetTitle("Vecty Tutorial")
   // Declare a new component
   c := &MyComponent{}
   // Render the component
   vecty.RenderBody(c)
}

// Our working component
type MyComponent struct {
    // To use it as a vect.Component
    vecty.Core
}

func (c *MyComponent) Render() *vecty.HTML {
   // Return a new elem.Body
   // It will also replace the existing body element of the target webpage
    return elem.Body(
        elem.Heading1(
            vecty.Text(
                "Hello, World",
            ),
        ),
    )
}
```
Since Vecty is built on gopherjs, we can view it right away with the gopherjs 
development server. Open a terminal and run `gopherjs server`, and navigate to 
the appropriate URL. The gopherjs server will render and serve this file 
directly from your **GOPATH**. For instance, if you saved **main.go** here:

 ```sh
$GOPATH/src/vecty-example/main.go
```

The development server should serve this page at `http://localhost:8080/vecty-example`

## Explanation

This code is very simple. We declare one custom Component, and this component
embeds the `Body`, `Heading1`, and `Text` elements built in to Vecty.
Using Components let's us create high-level, reusable containers for our UI. 

At a minimum, a valid Component must be a `struct` that
* declares a `Render` function
* embeds `vecty.Core`
The `Render` function is where we define the look and feel of our component. The `vecty.Core` bit uses Go's struct embedding to help our component implement the `vecty.Component` interface.

So far, we haven't written any HTML. All we have done is write our component and pass it to Vecty's `RenderBody` function. This function is special: you **must** pass it a Component that returns an `elem.Body`. Fortunately, our code satisfies
this.


# Vecty Components

> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation. This page provides an introduction to the idea of components. You can find a detailed component documentation [here](https://godoc.org/github.com/gopherjs/vecty).

Conceptually, components are like Golang functions. They accept arbitrary inputs (from struct fields) and return `vecty.ComponentOrHTML` describing what should appear on the screen.

## Declaring a component

To be a component, a struct must have the `vecty.Core` to be a `vecty.Component` interface.

```go
type MyComponent struct {
    vecty.Core
}
```
This struct also may contain extra fields. As:

```go
type MyComponent struct {
    vecty.Core
    Text string
}
```

## Rendering a component

### The `Render()` Method
A component must have the `Render()` method. This method returns the HTML (`vecty.ComponentOrHTML`) presentation of a component.

```go
func (v *MyComponent) Render() vecty.ComponentOrHTML {
   return elem.Heading1(
        // Remembered?
        // The MyComponent.Text?
        vecty.Text(v.Text),
    )
}
```

Done! Your component is ready

### The `vecty.RenderBody()`

Till now we have a `MyComponent` struct with `Render()` method. And it's a `vecty.Component`.

So now the question is **how to render a component?**

```go
func main(){
    c := &MyComponent {
        Text: "Hello World!",
    }
    vecty.RenderBody(c)
}
```

Voila!
We have rendered a component!

Niw if you are following the same building and serving steps as we've shown at our [Hello World](hello-world) example, you should see the rendered page at `http://localhost:8080/vecty-example/`

## Documentation
From the [godoc.org](https://godoc.org/github.com/gopherjs/vecty#Component):
```txt
type Component interface {
        // Render is responsible for building HTML which represents the component.
        //
        // If Render returns nil, the component will render as nothing (in reality,
        // a noscript tag, which has no display or action, and is compatible with
        // Vecty's diffing algorithm).
        Render() ComponentOrHTML

        // Context returns the components context, which is used internally by
        // Vecty in order to store the previous component render for diffing.
        Context() *Core

        // Has unexported methods.
}
    Component represents a single visual component within an application. To
    define a new component simply implement the Render method and embed the Core
    struct:

    type MyComponent struct {
        vecty.Core
        ... additional component fields (state or properties) ...
    }

    func (c *MyComponent) Render() vecty.ComponentOrHTML {
        ... rendering ...
    }
```
