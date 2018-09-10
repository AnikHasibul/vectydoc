## Hello World
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

### Explanation

This code is very simple. We declare one custom Component, and this component
embeds the `Body`, `Heading1`, and `Text` elements built in to Vecty.
Using Components let's us create high-level, reusable containers for our UI. 

At a minimum, a valid Component must be a `struct` that
* declares a `Render` function
* embeds `vecty.Core`
The `Render` function is where we define the look and feel of our component. The `vecty.Core` bit uses Go's struct embedding to help our component implement the `vecty.Component` interface.

So far, we haven't written any HTML. All we have done is write our component and pass it to Vecty's `RenderBody` function. This function is special: you **must** pass it a Component that returns an `elem.Body`. Fortunately, our code satisfies
this.

[Next page (Vecty components)](vecty-components)
