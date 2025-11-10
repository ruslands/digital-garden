
#### Language Resources

- [Go Official Website](https://go.dev/)
- [Go Tutorials on Tutorialspoint](https://www.tutorialspoint.com/go/index.htm)
- [Go by Example](https://gobyexample.com/)
- [Go Tutorials on W3Schools](https://www.w3schools.com/go/)
- [Microsoft Learn: Getting Started with Go](https://docs.microsoft.com/en-us/learn/modules/go-get-started/)
- [Microsoft Learn: Serverless Go](https://docs.microsoft.com/en-us/learn/modules/serverless-go/)

---

#### Go Environment Setup

**Path Setup**:  
```bash
export PATH=/usr/local/go/bin:$PATH
```

**Go Modules**:  
`GO111MODULE` is an environment variable used to enable or disable Go modules.

---

#### Jupyter Notebook for Go

You can run Go in Jupyter Notebooks using Gophernotes:

- [Gophernotes GitHub Repository](https://github.com/gopherdata/gophernotes)
- [Habr Blog: Go in Jupyter Notebooks](https://habr.com/ru/company/skillfactory/blog/556938/)

To install the Go kernel for Jupyter:
```bash
cp /Users/konovalov/go/src/github.com/gopherdata/gophernotes/kernel/* ~/Library/Jupyter/kernels/gophernotes
```

---

#### Go Project Management

- [Go Getting Started Tutorial](https://go.dev/doc/tutorial/getting-started)

**go.mod**: Used to track your code's dependencies.  
Initialize a new module:
```bash
go mod init example/hello
```

Tidy up dependencies:
```bash
go mod tidy
```

---

#### Go Commands

- **Run a Go Program**:
  ```bash
  go run hello.go
  ```
  or simply:
  ```bash
  go run .
  ```

- **Replace Dependencies in go.mod**:
  ```bash
  go mod edit -replace example.com/greetings=../greetings
  ```
#### Go Syntax and Best Practices

**:= Operator**:  
This is a shortcut for declaring and initializing a variable in one line.  
Example:
```go
message := fmt.Sprintf("Hi, %v. Welcome!", name)
```

The longer way:
```go
var message string
message = fmt.Sprintf("Hi, %v. Welcome!", name)
```
#### Go Packages and Modules

Go code is grouped into **packages**, and packages are grouped into **modules**.
#### Popular Go Libraries

**Gorilla/mux**: A powerful HTTP router and URL matcher for building Go web applications.
