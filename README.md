# Tutorial 2-5: Return greetings for multiple people

[Return greetings for multiple people](https://go.dev/doc/tutorial/greetings-multiple-people)

- 여러개의 name을 매개변수로 받는 Hello function을 만든다고 하자. 기존에 이미 publish된 example.com/greetings module이 존재할 때는 호환을 위해 여러 매개변수를 받는 새로운 함수를 작성하는 것이 좋다.

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
    
    // Hellos returns a map that associates each of the named people
    // with a greeting message.
    func Hellos(names []string) (map[string]string, error) {
    	// A map to associate names with messages.
    	messages := make(map[string]string)
    	// Loop through the received slice of names, calling
    	// the Hello function to get a message for each name.
    	for _, name := range names {
    		message, err := Hello(name)
    		if err != nil {
    			return nil, err
    		}
    		// In the map, associate the retrieved message with
    		// the name.
    		messages[name] = message
    	}
    	return messages, nil
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
    }
    ```
    
    - Go에서 map을 초기화하는 문법은 다음과 같다. `make(map[*key-type*]*value-type*)`
    For more about maps, see [Go maps in action](https://blog.golang.org/maps) on the Go blog.
        - 초기값과 함께 초기화 가능
            
            ```go
            commits := map[string]int{
                "rsc": 3711,
                "r":   2138,
                "gri": 1908,
                "adg": 912,
            }
            
            // empty map
            m = map[string]int{}
            ```
            
        - 조회시, 해당 키가 존재하지 않으면 각 type의 zero value 반환
        ex) int: 0, string: "", pointer, interface, map, slice, channel, funciton: nil
        - map의 key는 비교 가능한 자료형이어야 한다. In short, comparable types are boolean, numeric, string, pointer, channel, and interface types, and structs or arrays that contain only those types. Notably absent from the list are slices, maps, and functions; these types cannot be compared using `==`, and may not be used as map keys.
        - Concurrency:
            
            [Maps are not safe for concurrent use](https://go.dev/doc/faq#atomic_maps): it’s not defined what happens when you read and write to them simultaneously.
            
        - Iteration order
            
            When iterating over a map with a range loop, the iteration order is **not specified** and is **not guaranteed** to be the same from one iteration to the next. If you require a stable iteration order you must maintain a separate data structure that specifies that order.    
            
    - for loop 문에서 `range`를 사용하면 key(or index), value를 탐색할 수 있다.
    (python의 enumerate와 유사)

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
    
    	// A slice of names.
    	names := []string{"Gladys", "Samantha", "Darrin"}
    
    	// Request a greeting message.
    	message, err := greetings.Hellos(names)
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
    map[Darrin:Hail, Darrin! Well met! Gladys:Hi, Gladys. Welcome! Samantha:Hail, Samantha! Well met!]
    ```
