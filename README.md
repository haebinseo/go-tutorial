# Tutorial 2-4: Return a random greeting

[Return a random greeting](https://go.dev/doc/tutorial/random-greeting)

1. modify greetings/greetings.go file
    
    ```go
    package greetings
    
    import (
    	"errors"
    	"fmt"
    	"math/rand"
    	"time"
    )
    
    // Hello returns a greeting for the named person.
    func Hello(name string) (string, error) {
    	// If no name was given, return an error with a message.
    	if name == "" {
    		return "", errors.New("empty name")
    	}
    	// Create a message using a random format.
    	message := fmt.Sprintf(randomFormat(), name)
    	return message, nil
    }
    
    // init sets initial values for variables used in the function.
    func init() {
    	rand.Seed(time.Now().UnixNano())
    }
    
    // randomFormat returns one of a set of greeting messages. The returned
    // message is selected at random.
    func randomFormat() string {
    	// A slice of message formats.
    	formats := []string{
    		"Hi, %v. Welcome!",
    		"Great to see you, %v!",
    		"Hail, %v! Well met!",
    	}
    
    	// Return a randomly selected message format by specifying
    	// a random index for the slice of formats.
    	return formats[rand.Intn(len(formats))]
    	// Intn returns, as an int, a non-negative pseudo-random number
    	// in the half-open interval [0,n). It panics if n <= 0.
    }
    ```
    
    - `randomFormat`은 소문자로 시작하므로 해당 패키지에서만 접근 가능함을 기억하자. (not exported)
    - `[]string` : This tells Go that the size of the array underlying the slice can be **dynamically changed**.
    - Go는 프로그램 setup 시에, 전역변수 초기화 직후 `init` function을 자동으로 실행한다.
2. modify hello/hello.go file
    
    ```go
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
    	message, err := greetings.Hello("Gladys")
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
    
3. run
    
    ```bash
    $ go run .
    Great to see you, Gladys!
    
    $ go run .
    Hi, Gladys. Welcome!
    
    $ go run .
    Hail, Gladys! Well met!
    ```
