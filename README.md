# Tutorial 2-2: Call your code from another module

1. example.com/hello module 새로 만들기
    
    ```bash
    $ go mod init example.com/hello
    ```
    
    ```go
    package main
    
    import (
        "fmt"
    
        "example.com/greetings"
    )
    
    func main() {
        // Get a greeting message and print it.
        message := greetings.Hello("Gladys")
        fmt.Println(message)
    }
    ```
    
2. Edit the `example.com/hello` module to use your local `example.com/greetings` module.
    - For production use, you’d publish the `example.com/greetings` module from its repository (with a module path that reflected its published location), where Go tools could find it to download it.
    - For now, because you haven't published the module yet, you need to adapt the `example.com/hello` module so it can find the `example.com/greetings` code on your local file system.
    
    2-1. use the `[go mod edit` command](https://go.dev/ref/mod#go-mod-edit) to edit the `example.com/hello` module to redirect Go tools from its module path (where the module isn't) **to the local directory** (where it is).
    
    ```bash
    # From the command prompt in the hello directory, run the following command:
    $ go mod edit -replace example.com/greetings=../greetings
    ```
    
    2-2. From the command prompt in the hello directory, run the `[go mod tidy` command](https://go.dev/ref/mod#go-mod-tidy) to **synchronize** the `example.com/hello` module's dependencies, adding those required by the code, but not yet tracked in the module.
    
    ```bash
    $ go mod tidy
    go: found example.com/greetings in example.com/greetings v0.0.0-00010101000000-000000000000
    ```
    
3. run
    
    ```go
    $ go run .
    Hi, Gladys. Welcome!
    ```
