---
title: "Devlog 3 Go Pointers Basics"
date: 2023-03-14T01:49:00Z
draft: false
tags: ['devlog', 'golang', 'go', 'pointers']
categories: ['Devlog', 'Go']
---

![img1](/images/devlog-1/img1.png)

Today I am going to run over pointers and how we use them within functions in Go. 
If read the post last week I had some questions at the bottom for you to answer.
Here they are:

1. What is an if statement, and how does it work?
	An 'if' statement uses boolean expressions to conditionally execute blocks of
	code based on if the expressions is true or false.
2. What is a for loop, and how does it work?
	A for loop will execute a specific block of code repeatedly until
	a certain conidition is met.
3. What does the import fmt do?
	Apart of the Go standard library 'fmt' provides functions for formatting and 
	printing text.
4. os Stands for?
	'os' stands for 'Operating System'. This is also another package within the 
	standard library.
5. Where can you find a list of the standard libraries for Go?
	https://golang.org/pkg/

Topics included:

- [What Are Pointers](#what-are-pointers)
- [Why Do We Need Them](#why-do-we-need-them)
- [How Do We Create Pointers](#how-do-we-create-pointers)
- [Passing by Reference in Go Functions](#passing-by-reference-in-go-functions)
- [End](#end)
- [Questions](#questions)

---

### What Are Pointers

Within programming, pointers are variables that contain memory addresses. These addresses point to specific data stored in memory. We use pointers to manipulate this data indirectly.

### Why Do We Need Them

When manipulating data within Go through a function or a method, you will pass the data as an argument. By passing the variable as an argument, Go creates a copy of that variable's value. The function will not manipulate the original variable's value. Instead, it will make changes to the new copy within the function that Go, by default, will create. This is known as 'passing by value'.

In situations where you need to modify the value of a variable inside a function or method, using pointers is often the best approach. By passing a pointer to the variable, you can directly manipulate the value stored at the memory location pointed to by the pointer, rather than working with a copy of the variable's value. This approach is known as 'passing by reference', as you are only referencing where that variable is stored within memory.

Another common use case for pointers is when working with large data sets. When manipulating large data sets without using pointers, the process can consume a significant amount of time and memory. By using pointers, you can avoid creating unnecessary copies of the data, which can improve performance and reduce memory usage.

### How do we create pointers?

To create a pointer, we use the '&' ampersand in front of a variable. This asks to get the memory address where that variable is stored. If we declare it explicitly, we must also make sure our pointer type is the same as the type of the variable that we are pointing to. Using the shorthand syntax, the Go compiler will assume the type that has been assigned to the variable it is being asked to create a pointer for.

```go 
package main

import "fmt"

func main()  {

	var item string = "bread"

	// Two ways to declare pointers.
	var ePointer *string = &item // explicit declaration
	pointer := &item             // Short hand, using Go compiler to infer the type.

	fmt.Println(item)     // bread
	fmt.Println(ePointer) // 0xc00010a020
	fmt.Println(pointer)  // 0xc00010a020
}

```

If we want to derefernce that address we can use the deferencing operator 
'*' asterisk. 
```go 
package main

import "fmt"

func main()  {

	var item string = "bread"
    var pointer *string = &item

	fmt.Println(item)     // bread

	// Deferencing a pointer to show the value.
	fmt.Println(*pointer) // bread
}
```

### Passing by Reference in Go Functions 
So as I stated above when we just pass a variable through a function in Go 
we're not manipulating the actual variables data. We're 'passing by value' meaning Go will
create a copy of that variable and then manipulate the copy. Sometimes we want that but other
times we want to manipulate the value associated with that variable so we create a function
that we can pass a refrence to. In the example below I've demostrated how 'Passing by Value'
and 'Passing by Reference' work in relation to functions.

```go 
package main

import "fmt"

// Function that takes int arugment by value.
func passByValue (x int) {
	// Add 2 to the argument (which is a copy of the original value).
	x += 2
} 

// A function that takes an int argument by reference (pointer).
func passByReference (y *int) {
	// Add 2 to the arugment stored at the memory address pointed to by y.
	*y += 2
}


func main()  {
	// Two variables with int values of 2.
	x := 2
	y := 2 

	// Pass by value.
	fmt.Println("Before function call:", x) // 2
	passByValue(x)
	fmt.Println("After function call:", x)  // 2

	// Pass by reference.
	fmt.Println("Before function call:", y) // 2
	passByReference(&y)
	fmt.Println("After function call:", y)  // 4
}
```

### End
Hopefully in this small devlog you've gained a better understanding of the
basics of Go pointers. In the coming devlogs I will dive deeper into them 
this was just to introduce them.

### Questions 

	1. What is meant by 'Pass by Value'?
	2. What is meant by 'Pass by Reference'?
	3. How do you declare a pointer type?
	4. How do you create a function that takes a reference?
	5. Take the last code block and modify it to use methods.

