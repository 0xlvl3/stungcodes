---
title: "Devlog 1 Hello Golang"
date: 2023-02-19T18:45:02+11:00
draft: false
tags: ['devlog', 'golang', 'go', 'hello-world']
categories: ['Devlog', 'Go']
---

![img1](/images/devlog-1/img1.png)

Greetings! This is the first post in my series documenting my journey of learning Go (or Golang) from scratch. I have previous experience in Python and feel confident using it. However, my motivation for learning Go is to enhance my skills in developing web servers, microservices, and APIs.

Topics covered:
- [First Go Program ](#first-go-program)
- [Breakdown of the Code](#breakdown-of-the-code)
- [Difference Between Package and Import](#difference-between-package-and-import)

### First Go Program

I thought I'd kick it off with a hello-world program.

>```go
>//hello-devlog.go
>package main
>
>import "fmt"
>
>func main(){
>	fmt.Println("Hello Devlog!")
>}
>```

>```
>go run hello-devlog.go
>Hello Devlog!
>```

### Breakdown of the Code

Let's work through each of these:

-   `package main` - The `main` package is a special package in Go that is used to create executable programs. Any Go program must have the `main` package.
-   `import "fmt"` - When we `import`, we import external packages into our Go program. `fmt` is a package provided to us from the standard library. `fmt` provides functions for formatted input and output.
-   `func main()` - `func` in Go is a keyword short for function. We use `func` to declare new functions. `func main()` is a special function within Go. When we run our Go program, it will be the first function that is executed. Our Go program can only ever have one `main()` since it is used to handle the program's state, call other functions, and handle errors.
-   `fmt.Println()` - `Println()` is a function provided by our `fmt` import. It is similar to `print` in other programming languages. It is useful for debugging output to the console.
-   `go run hello-devlog.go` - To execute and compile our code within the terminal, we have to use `go run filename.go`. Using `go run` is the quick way to automatically compile the program and execute it immediately.

### Difference Between Package and Import

After reading the above, you might be wondering, "What's the difference between `package` and `import`?" In a way, calling `package` is like requesting a toolbox that can contain many functions and types designed to perform a specific task. On the other hand, `import` is like requesting a specific tool from that toolbox (i.e., package) that we need to accomplish a particular job in our program.

On that I'll conclude this devlog, hope you got some value till next time.
