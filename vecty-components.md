# Vecty Components

> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation. This page provides an introduction to the idea of components. You can find a detailed component documentation [here](https://godoc.org/github.com/gopherjs/vecty).

Conceptually, components are like Golang functions. They accept arbitrary inputs (from struct fields) and return `vecty.ComponentOrHTML` describing what should appear on the screen.

### Initializing a component

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