# Tutorial 2-1: Create a Go module

[Tutorial: Create a Go module](https://go.dev/doc/tutorial/create-module)

```go
package greetings

import "fmt"

// Hello returns a greeting for the named person.
func Hello(name string) string {
    // Return a greeting that embeds the name in a message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```

- Declare a `greetings` package to collect related functions.
- In Go, a function whose name starts with a capital letter can be called by a function not in the same package. This is known in Go as an **exported name**. For more about exported names, see [Exported names](https://go.dev/tour/basics/3) in the Go tour.
- In Go, the `:=` operator is a shortcut for declaring and initializing a variable in one line (Go uses the value on the right to determine the variable's type). Taking the long way, you might have written this as:
    
    ```go
    var message string
    message = fmt.Sprintf("Hi, %v. Welcome!", name)
    ```