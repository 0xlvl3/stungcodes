---
title: "Devlog 4 Structs Part 1"
date: 2023-03-15T09:26:04Z
draft: false
tags: ['devlog', 'golang', 'go', 'structs', 'types']
categories: ['Devlog', 'Go']
---

Welcome back to the next installment of my devlog. 
	

### Last devlog questions: 

	1. What is meant by 'Pass by Value'?
	2. What is meant by 'Pass by Reference'?
	3. How do you declare a pointer type?
	4. How do you create a function that takes a reference?
	5. Take the last code block and modify it to use methods.

### Topics included:
- [What Are Structs](#what-are-structs)
	- [Defining Basic Structs](#defining-basic-structs)
	- [Instantiating Structs](#instantiating-structs)
	- [Accessing Fields Within Structs](#accessing-fields-within-structs)
- [Comparison to Python Classes](#comparison-to-python-classes)
	- [Class and Struct](#class-and-struct)
	- [Methods](#methods)
- [End](#end)
- [Questions](#questions)
- [Primitive Types in Go](#primitive-types-in-go)

---

### What Are Structs?

Short for "structure", we use structs to group different types of data under one defined variable, which are known as composite data types. Within our struct, we define a field with a name and a type. For a field, Go can take any of the primitive types such as string, bool, float32, int, and more. I will leave a list at the end [here](#primitive-types-in-go). Go can also take user-defined types within a struct; these are defined using the keyword type.

#### Defining Basic Structs

When we define a struct, we use the type keyword, variable name, struct, followed by "{}". Within our curly brackets, we define our fields.


```go 
package main

import "fmt"

// Creating a basic BankAccount Struct.
type BankAccount struct{
	// Fields for our BankAccount Struct.
	// name type
	FirstName string
	LastName string
	Balance float32
}

```

#### Instantiating Structs

Having defined our struct, we can now create instances of that struct. There are two ways to do this: by using the literal syntax, where we provide values for each field, or by using the new keyword, which will return a zero-valued instance of the struct.


```go 
package main

import "fmt"

// Creating our BankAccount struct.
type BankAccount struct{
	// Creating our fields.
	// name type
	FirstName string
	LastName string
	Balance float32
}

func main()  {

	// Creating a BankAccount instance using struct literal syntax. 
	account := BankAccount{FirstName: "Jon", LastName: "Snow", Balance: 10000}

	// Creating a BankAccount using new keyword.
	account2 := new(BankAccount) // Returns a pointer to a zero-value instance of struct.

	// Outputs next to statement.
	fmt.Println("Account 1:", account) // {Jon Snow 10000}
	fmt.Println("Account 2:", account2) // &{ 0}
	fmt.Println("Account 2:", *account2) // { 0}	
}
```

#### Accessing Fields Within Structs 


Once we have created our structure within our program and filled the fields required, we can then make calls to those specific fields. In the example below, I have tried to create fields of the basic primitive types. You can find the list of primitive types used in Go [here](#primitive-types-in-go). Notice in this code block also that when I defined my struct, I grouped the variables related to the same type and separated them by a comma. When we access a field in a struct, we use dot notation, as shown below. Using dot notation, we can also modify the fields of the specified struct.

```go 
package main

import "fmt"

// Creating our BankAccount struct.
// Notice here when I created my struct I put the same types separated by ','
type BankAccount struct{
	// Creating our fields.
	// name type
	FirstName, LastName, Address string
	BirthDay, BirthMonth, BirthYear int
	Loan bool
	Balance, LoanBal float32
}

func main()  {

	// Creating a BankAccount instance filling the data as required.
	account123 := BankAccount{
		FirstName: "Jon",
		LastName: "Snow",
		Address: "Winterfell st",
		BirthDay: 01,
		BirthMonth: 03,
		BirthYear: 1990,
		Loan: false,
		Balance: 10000,
	}

	// Examples of using struct data. Output is commented below each block.
	fmt.Printf("Name: %s %s is born: %d/%d/%d\n", 
		account123.FirstName, 
		account123.LastName, 
		account123.BirthDay, 
		account123.BirthMonth, 
		account123.BirthYear)
	// Name: Jon Snow is born: 1/3/1990

	fmt.Printf("Does %s %s have a loan? %t\n", 
		account123.FirstName, 
		account123.LastName, 
		account123.Loan)
	// Does Jon Snow have a loan? false

	fmt.Printf("Hello %s %s your balance is: $%f\n", 
		account123.FirstName,
	    account123.LastName,
	    account123.Balance)
	// Hello Jon Snow your balance is: $10000

	fmt.Printf("Your address is: %s\n", account123.Address)
	// Your address is: Winterfell st

    // Modifying a field.
	fmt.Println(account123.Balance) // 10000
	account123.Balance = 0 
	fmt.Println(account123.Balance) // 0
}
```

Using the other method of zero-value structs:
```go 
package main

import "fmt"

// Creating our BankAccount struct.
type BankAccount struct{
	// Creating our fields.
	// name type
	FirstName string
	LastName string
	Address string
	Loan bool
	Balance float32 
	LoanBal float32
}

func main()  {

	// Creating a BankAccount instance using zero-value struct,
	// then filling the fields with dot notation.
	account123 := new(BankAccount)
	account123.FirstName = "Jon"
	account123.LastName = "Snow"

	fmt.Println("You name is: ", 
		account123.FirstName, 
		account123.LastName)
	// Your name is: Jon Snow

}
```
We can also place structs within structs. These structs are known as 'user-defined types', which can improve maintainability, readability, reusability, and encapsulation. The following example shows how we accomplish this:

```go 
package main

import "fmt"

// Creating our BankAccount struct.
type BankAccount struct{
	// Creating our fields.
	// name type
	FirstName string
	LastName string
	Address string
	Loan bool
	Balance float32 
	LoanBal float32
	// Our last two are user defined structs.
	// Notice our type.
	DOB Birthday
	Phone PhoneNumber
}

// Our user defined structs.
type Birthday struct{
	Day int 
	Month int
	Year int
}

type PhoneNumber struct {
	Home int
	Work int
}

func main()  {
	// Creating an instance of BankAccount
	account123 := BankAccount{
		FirstName: "Jon",
		LastName: "Snow",
		Address: "Winterfell st",
		Loan: false,
		Balance: 10000,
		// Here we use the same syntax we use to instance a struct just inside our 
		// struct we are instantiating.
		DOB: Birthday{	
			Day: 01,
			Month: 03,
			Year: 1990,
		},	
		Phone: PhoneNumber{
			Home: 123456789,
			Work: 987654321,
		},
	}

	// We can then access it using the dot notation like before.
	fmt.Println(account123.DOB.Year) // 1990
	fmt.Println(account123.Phone) // {123456789 987654321}

}

```

In the section below, I have compared Go to Python classes and how they are quite similar. If you're coming from another language, the next section might be helpful for you if you don't know Python. Feel free to skip to the next part. If you want to read it anyways. Enjoy.


---
### Comparison to Python Classes

#### Class and Struct

If you're coming from another language, you can compare Go structs to other composite data types. I have chosen to show Python classes, Go structs, and Python classes, as they are very similar constructs. You can declare the struct and class, then pass to it fields that have a name and type. See below.


Python:
```python
# Defining a Python class.
class BankAccount:
    # init fields are required when initialised.
    def __init__(self, first_name, last_name, balance):
        # Feilds required.
        self.first_name = first_name
        self.last_name = last_name
        self.balance = balance


# Initialization of an object.
account = BankAccount("Jon", "Snow", 10000)

print(account.first_name, account.last_name, account.balance)  # Returns Jon Snow 10000.
```

Go:
```go
package main

import "fmt"

// Creating our BankAccount struct.
type BankAccount struct{
	// Creating our fields.
	// name type
	FirstName string
	LastName string
	Balance float32
}

func main(){
	// Initialization of our new struct.
	account := BankAccount{FirstName: "Jon", LastName: "Snow", Balance: 10000.00}

	fmt.Println(account.FirstName, account.LastName, account.Balance) // Jon Snow 10000
}
```
#### Methods

Python classes will also take methods, which are functions that can be called on instances of the class. In Go, you similarly create specific functions which are methods associated with Go structs. These methods can only be called with that specific type in Go, which corresponds to the struct. If you're unfamiliar with methods, think of them as doing actions; that's how I remember them.

Python:
```python
# Defining a Python class.
class BankAccount:
    # init fields are required when initialised.
    def __init__(self, first_name, last_name, balance):
        # Feilds required.
        self.first_name = first_name
        self.last_name = last_name
        self.balance = balance

    # Creating our method for our BankAccount class.
    def printBalance(self):
        print(
            f"""
Hello {self.first_name} {self.last_name} your current bank balance is: ${self.balance}
Have a great day!"""
        )


# Initialization of an object.
account = BankAccount("Jon", "Snow", 10000)

# Calling our method from our object.
account.printBalance()
# Output:
# Hello! Jon Snow your current balance is: $10000.000000
# Have a great day!
```

Go:
```go 
package main

import "fmt"

// Creating our BankAccount struct.
type BankAccount struct{
	// Creating our fields.
	// name type
	FirstName string
	LastName string
	Balance float32
}

// printBalance can be called on a BankAccount type which then it will print the balance of that account.
func (ba BankAccount) printBalance() {
	fmt.Printf("Hello! %s %s your current balance is: $%f\nHave a great day!\n", ba.FirstName, ba.LastName, ba.Balance)
}

func main() {
	// Initialization of our new struct.
	account := BankAccount{FirstName: "Jon", LastName: "Snow", Balance: 10000.00}

	// Calling the printBalance method on an account.
	account.printBalance()
	//Output:
	//Hello! Jon Snow your current balance is: $10000.000000
	//Have a great day!
}
```
---

### End

Now we have seen how we can create a struct, create an instance of that struct, 
set data to that instance, get data from that instance, manipulate that data and 
create user defined types within structs. In the next blog I will visit this topic 
again and we will dive deeper into structs with functions and methods. Till next time!


### Questions

	1. What are the basic primitive types in go?
	2. What are user defined types?
	3. How do you declare a struct?
	4. How can you manipulate a field on a struct?
	5. Create a struct that takes a user defined type.

### Primitive Types in Go 

	- Numeric Types:
		- Integers: 
			- Signed integers: `int8`, `int16`, `int32`, `int64`, `int`
			- Unsigned integers: `uint8`, `uint16`, `uint32`, `uint64`, `uint`
		- Floating-point numbers: `float32`, `float64`
		- Complex numbers: `complex64`, `complex128`
		- Byte (alias for uint8): `byte`
	- String Types:
		- `string`
	- Boolean Types:
		- `bool`
	- Special Types:
		- Rune (alias for int32, represents a Unicode code point): `rune`
