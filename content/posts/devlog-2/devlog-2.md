---
title: "Devlog 2"
date: 2023-02-25T12:47:26+11:00
draft: false
---
Welcome to the second installment of my Devlog. Learning the basics of Golang I thought I would run through some of the code that I've just gone through and explain each part. If you haven't read the first some of the code that will be in todays blog was in that you can find it here.

Topics included:

- [User Input, If Else Statements and For Loops](#user-input,-if-else-statements-and-for-loops)
	- [Code Block 1 Breakdown](#code-block-1-breakdown)
- [Creating and Writing to a Text File](#creating-and-writing-to-a-text-file)
	- [Code Block 2 Breakdown](#code-block-2-breakdown)
- [Reading a Text File](#reading-a-text-file)
- [End](#end)
- [Questions](#questions)

---

Repo for this blogs code can be found here: https://github.com/stungnet/devlog-2-code.git

### User Input, If Else Statements and For Loops

I put together this example to run through basics that could be used to build fundamentals off. Understanding how to take in user input and use it, navigate if else statements and how for loops work can you essential building blocks to any program. These are at the core of any language that you decide to learn.

_Within this first code block I will go into everything, in the subsequent blocks I won't expand on duplicate code._

Before looking over our first code block, I would quickly like to introduce `if statements` and `for loops`. I believe having an understanding of them first before we dive into the breakdown will help.

`if else statements` - An if statement is a conditional structure which allow you to execute specific blocks of code based on a given condition. You can add an additional conditional to an if statement, with `else if` followed by the condition. To finish off most `if` statements you will end it with a `else` clause for if all other conditionals aren't meant this will usually be some sort of default action that will happen. The `else` block is not mandatory to have, you can run `if` statements just by themselves, as you can see in the first code block I've put together.
>```go
>// If statement.
>if condition { 
>// code to be executed if condition is true 
>}
>
>// If else statement.
>if condition { 
>// code to be executed if condition is true 
>} else { 
>// code to be executed if condition is false 
>}
>
>// Else if statement.
>if condition1 { 
>// code to be executed if condition1 is true 
>} else if condition2 { 
>// code to be executed if condition2 is true and condition1 is false 
>} else { 
>// code to be executed if condition1 and condition2 are false 
>}
>
>```

`for loops` - For loops are control structures that allow you to execute a block of code repeatedly based on a condition. In the code below we are using an infinite for loop, with a means to `break` out of that loop with conditional statements when given input it's expecting, in this case `a` or `b`. The use of conditional `else if` statements here make it a finite loop. Without a way to `break` or exit the for loop, an infinite loop will loop indefinitely meaning the only way out of this loop is to cancel execution of the program.
```go
// For loop with no initialization, condition or post statements. This loop can only be broken with a break statement or the program is terminated. (what we used above)
for {
	// code to be executed
}

/* 
Standard loop form.
---
- initialization statement will be executed before the first iteration
- condition expression is evaluated before each iteration
- post statement executed after each iteration
*/
for initialization; condition; post {
	// code to be executed
}
```

Now we have looked over `if` statements and `for loops` let's move onto the first block. In this block we are taking some input from the user and then putting it to different conditionals. Not entering anything when prompted, the program will exit. While entering `a` or `b` you will get given a sentence and break out of the loop. If you don't answer `a` or `b` you will be prompted again until you answer correctly.

```go
package main //Main package that will create executable

// Imports from standard library
import (
	"fmt" // Format and print functions for outputting data to console.
	"os" // Operating system functions.
	"strings" // String manipulation functions.
)

// Main function is our entry point for the program where our code begins execution when executed.
func main(){
	var input string // Our string which user input will be stored.
	
	// The beginning of our for loop
	for {
		// Ask user question with fmt.Printf, then take the input with fmt.Scanln.
		fmt.Printf("Please enter 'a' or 'b': ")
		fmt.Scanln(&input)

		// If statement will check that user has not enter just blank. This will only check the first entry not subsequent.
		if len(strings.TrimSpace(input)) == 0{
			fmt.Println("You didn't enter anything? ¯(°_o)/¯")
			os.Exit(1)
		}

		// We take that use input and convert it to lowercase.
		userInputLower := strings.ToLower(input)

		// Let the user know what they entered.
		fmt.Println("You entered", userInputLower)

		// If block will check to see if "a" or "b" if neither user will be asked again until it is "a" or "b".
		if userInputLower == "a"{
			fmt.Println("You listened!")	
			break
		} else if userInputLower == "b"{
			fmt.Println("Why did you go b??")
			break
		} else{
			fmt.Println("Input incorrect try again.")
		}
	}
}
```

#### Code Block 1 Breakdown
- `package main` - The `main` package is a special package in Go that is used to create executable programs. Any Go program must have the `main` package.
- `import` - Here we are importing some functions from the standard Go library. Search for any import here https://pkg.go.dev/
- `fmt` - Format functions are for formatting and printing output to the console.
- `os` - Operating System or `os` is a package in Go that provides functions for interacting with the operating system.
- `strings` - All manipulations to strings from the standard library are within the `strings` functions package, this can be anything from `ToLower()`, `ToUpper()`, `Split()` or in the example above `TrimSpace`.
- First `if` statement - Here we use `len` the `len` function will check the length of a given string. Using our imported functions from `strings` we call `TrimSpace` onto the input. This will remove all whitespace within a string, if the user enters no input in `input` then `len` with `TrimSpace` will return 0. This is a way to check before continuing the loop, if input = 0 we will tell the user they entered nothing and exit with `os.Exit()`. Using the `os` import with the `Exit` function terminates a running program and returns an exit code, here we pass 1 so if it exits here we know where it did.
- `strings.ToLower` - Here we use the functions within `strings` again to lowercase our text. We do this to make sure that if the user even entered `A` it will still become `a`. This is useful in case-insensitive matters.
- `else if` statement - In the last block of conditionals, we check to see if user entered `a` or `b`. If they do, we break out of the loop with either message. If they don't correctly enter `a` or `b` It will prompt the user again since our `else` clause doesn't break our loop.


---
### Creating and Writing to a Text File

Code block 2 we are creating a file, in this case a .txt, then writing something to that file. This can be handy to know how to do, and it is pretty common to learn how to manipulate files by writing and reading. 

The comments throughout the code hopefully should give a good understanding of what is happening at each step. In the breakdown, I want to talk about `bufio`.

_To note I won't be explaining previous things explained in the last example._
```go
package main // Main package that will create executable

import (
"bufio" // Buffered input and output opertations; Improve reading and writing.
"fmt" // Format and print functions for outputting data to console.
"os" // Operating system functions
)

// Main function is entry point for the program, where code begins execution when executed.
func main(){
var filename string // String to store new filename
fmt.Printf("Give your new .txt file a name: ") // Prompt user.
fmt.Scan(&filename) // Take user input.

// Here we add .txt to the users inputted filename.
filename += ".txt"

// Creating filename.txt
os.Create(filename)

fmt.Printf("%v created!", filename) // Let user know it has being created.

// Prompt user to enter text to our new filename.txt
fmt.Println("What do you want to write to the file? ")

// Create a new bufio.Reader that reads from os.Stdin, which is our users input.
scanner := bufio.NewReader(os.Stdin)

// Using the `scanner` we created just before we ReadString until the end of the line and assign it to contents.
contents, _ := scanner.ReadString('\n')

// Convert the input to bytes to write it to the file.
data := []byte(contents)
os.WriteFile(filename, data, 0664)
}
```

#### Code Block 2 Breakdown

`bufio` is a Go package that provides buffered input and output functionality for reading and writing data. This means that `bufio` can store data in memory and allow you to read or write it in chunks, which can be more efficient than reading or writing data byte-by-byte. To use `bufio`, you typically start by creating a `reader` variable to read data, like so:

```go
scanner := bufio.NewReader(os.Stdin)
```

Here, we use `bufio` in conjunction with `os.Stdin`, which represents the standard input stream for our program. This creates a `reader` that can be used to read data from the standard input.

Then, to read the contents of the `scanner` variable, we use the `ReadString()` method with the delimiter `\n`, which means to read the string until a new line character is encountered. In our example, we store the contents of the scanner in a variable called `contents` like this:

```go
contents, _ := scanner.ReadString('\n')
```

Capturing those contents within our `contents` variable now we want to write that string to the file. To do that in Go, we convert our string to a slice of bytes using `[]byte()` function. Converting to a slice of bytes, we then pass those bytes to `os.WriteFile` function which takes a filename as a string, data consisting of a slice of bytes and permissions to be set onto that file.

```go
data := []byte(contents)
os.WriteFile(filename, data, 0664)
```

---


### Reading a Text File

For this code block go check the repo we are going to use a small `txt` file from there. We are just going to read the contents of that file through our program. 

_If you downloaded the repo, you would have noticed already that the executable binaries were included within write and read. If you want to follow along here and build it yourself just delete the main executable._

For this program let's compile our code using `go build`. This command is used to compile Go source code files. It will take as many source files as it's passed and then compile them into an executable binary file as output.

gif 1

Here you can see me first checking the directory and seeing the contents with `ls`. After seeing what is in our directory, we run the `go build` command on our `main.go` file. This generates a new file called `main` this is our executable binary file. To run this file we use the `./` followed by the filename in this case it would be `./main`.

gif 2

This will be common practice for our files so from now on when I am running files within this devlog, I will be building them and then running them.

Everything we use within this block we have covered, try and write your own break down of what each part of the code does as an execerise.

```go
package main // Main package that will create executable

import (
"fmt" // Format and print functions for outputting data to console.
"os" // Operating system functions.
)

// Main function is entry point for the program, where code begins execution when executed.
func main (){

// filename will store user input.
var filename string

// Ask for user input.
fmt.Printf("Name the txt filename contents you want to read: ")

// Store user input.
fmt.Scan(&filename)
filename += ".txt" // add .txt extension to user input.

// Here we take the filename given with the added .txt and look for that file to read.
data, err := os.ReadFile(filename)
if err != nil {
fmt.Println(err) // If file doesn't exist it will throw error.
os.Exit(1) // Exit program with error 1.
}

// If filename exists we take that data and place it into a content variable then print it to the user.
content := string(data)
fmt.Println(content)
}

```

---

### End

With all that, hopefully you have an understanding of some of the Go basics. Below I have some questions for you to answer so you can put some of the concepts into your own words, I will post the answers within the next devlog. Thanks for taking the time to read my devlog and have a great day/night. 

#### Questions
1. What is an if statement, and how does it work?
2. What is a for loop, and how does it work?
3. What does the import `fmt` do?
4. `os` Stands for?
5. Where can you find a list of the standard libraries for Go?

