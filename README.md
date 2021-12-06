# Tutorial 2-3: Return and handle an error

1. modify greetings/greetings.go file
    
    ```go
    // greetings.go 
    package greetings
    
    import (
        "errors"
        "fmt"
    )
    
    // Hello returns a greeting for the named person.
    func Hello(name string) (string, error) {
        // If no name was given, return an error with a message.
        if name == "" {
            return "", errors.New("empty name")
        }
    
        // If a name was received, return a value that embeds the name
        // in a greeting message.
        message := fmt.Sprintf("Hi, %v. Welcome!", name)
        return message, nil
    }
    ```
    
    - Any Go function **can return multiple values**
    - `nil` means no error
2. modify hello/hello.go file
    
    ```go
    // hello.go
    package main
    
    import (
        "fmt"
        "log"
    
        "example.com/greetings"
    )
    
    func main() {
        // Set properties of the predefined Logger, including
        // the log entry prefix and a flag to disable printing
        // the time, source file, and line number.
        log.SetPrefix("greetings: ")
        log.SetFlags(0)
    
        // Request a greeting message.
        message, err := greetings.Hello("")
        // If an error was returned, print it to the console and
        // exit the program.
        if err != nil {
            log.Fatal(err)
        }
    
        // If no error was returned, print the returned message
        // to the console.
        fmt.Println(message)
    }
    ```
    
    - Use the functions in the standard library's `log package` to output error information. If you get an error, you use the `log` package's `[Fatal` function](https://pkg.go.dev/log?tab=doc#Fatal) to **print the error and stop the program**.
3. run and check an error
    
    ```bash
    $ go run .
    greetings: empty name
    exit status 1
    ```

That's common error handling in Go: **Return an error as a value** so the caller can check for it.
