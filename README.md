# Tutorial 2-7: Compile and install the application

[Compile and install the application](https://go.dev/doc/tutorial/compile-install)

- `go build`
    
    The `[go build` command](https://go.dev/cmd/go/#hdr-Compile_packages_and_dependencies) compiles the packages, along with their dependencies, but it doesn't install the results.
    
    - build하면 해당 디렉토리에 hello file이 생성됨. 결과물은 다음과 같이 실행 가능
        
        ```bash
        $ ./hello
        map[Darrin:Great to see you, Darrin! Gladys:Hail, Gladys! Well met! Samantha:Hail, Samantha! Well met!]
        ```
        
    - 현재 결과물을 실행하기 위해서는 결과물이 있는 폴더나 경로를 알아야만 한다.
- `go install`
    
    The `[go install` command](https://go.dev/ref/mod#go-install) compiles and installs the packages.
    
    - install path가 system의 shell path에 등록되어 있지 않다면 추가해준다.
        
        ```bash
        # Go install path 확인
        $ go list -f '{{.Target}}'
        /Users/seohaebin/go/bin/hello
        # /Users/seohaebin/go/bin가 install path이다.
        
        # 환경변수 추가
        $ export PATH=$PATH:/path/to/your/install/directory
        
        # 반대로 내가 원하는 경로로 install path 수정도 가능
        $ go env -w GOBIN=/path/to/your/bin
        ```
        
    - 설치
        
        ```bash
        $ go install
        
        # 어느 경로에서든 실행 가능
        $ hello
        map[Darrin:Hail, Darrin! Well met! Gladys:Great to see you, Gladys! Samantha:Hail, Samantha! Well met!]
        ```