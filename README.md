# Tutorial: Get started with Go

1. Enable dependency tracking for your code.
    - To enable dependency tracking for your code by creating a go.mod file, run the `go mod init` command
    
    ```bash
    $ go mod init example/hello
    go: creating new go.mod: module example/hello
    ```
    
    - module name에 대한 추가 자료:
    [https://go.dev/doc/modules/managing-dependencies#naming_module](https://go.dev/doc/modules/managing-dependencies#naming_module)
2. hello.go 파일 작성
    
    ```go
    // hello.go
    package main
    
    import "fmt"
    import "rsc.io/quote"
    
    func main() {
        fmt.Println(quote.Go())
    }
    ```
    
3. Add new module requirements and sums
    - Go will add the `quote` module as a requirement, as well as a go.sum file for use in authenticating the module.
    - 기본적으로 새로운 모듈 추가 시 hash 값 비교를 통해 변조 여부를 확인한다. ⇒ authenticating
    - For more, see [Authenticating modules](https://go.dev/ref/mod#authenticating) in the Go Modules Reference.
    
    ```bash
    $ go mod tidy
    go: finding module for package rsc.io/quote
    go: found rsc.io/quote in rsc.io/quote v1.5.2
    ```
    
4. run the code
    
    ```bash
    $ go run .
    Don't communicate by sharing memory, share memory by communicating.
    ```
    
    ※ 메시지만 한 줄 뜨고 종료되는 것이 맞음.