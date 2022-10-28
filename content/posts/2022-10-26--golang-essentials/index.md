---
title: Golang essentials
date: "2022-10-26T12:40:32.169Z"
template: "post"
draft: false
slug: "golang-essentials"
category: "Golang"
tags:
  - "Golang"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Go was designed to run on multiple cores and built to support concurrency. Concurrency in Go is cheap and easy via Go routines.

Go is a perfect fit for performant applications which are running in scalable and distributed systems.

Characters of Go:
- Efficiency and safety like c++
- Fast build time, start up and run
- Simple Syntax: Easy to learn, read and write
- Require fewer resources
- Compiles into single binaries

## Install Go
- Download Go package via https://go.dev/
- Install Go addon in VSCode

## Init Go repository

```bash
go mod init my-go-app
```

This would initialized a `go.mod` file. This file describes module path and go version

## First go file and entry point

Create a main.go file and write below in the first line:
```go
package main

import "fmt"

func main() {
   fmt.Printf("Hello World %v!\n", "Bailey")
}
```

## Run go program
Execute below code
```shell
go run main.go
```

## Variable

```go
var bookingCount unit = 50
// or
bookingCount  := 50

var status bool = true

var name string
var currentCount uint
```

## Constant
```go
const bookingCount unit = 50
const appName string = "Go APP"
```

## Get inputs via pointers
```go
var firstName string
var lastName string
var email string
var userTickets uint

// ask user for their name (pointer)
fmt.Println("Enter your first name:")
fmt.Scan(&firstName)

fmt.Println("Enter your last name:")
fmt.Scan(&lastName)

fmt.Println("Enter your email:")
fmt.Scan(&email)

fmt.Println("Enter tickets:")
fmt.Scan(&userTickets)
```


## Array and slice

```go
// array - Fixed size
var bookings = [5]string{"a","b","c"}
//or
var bookings [5]string

bookings[3] = "d"

// slice - dynamic size
var bookings []string
// or
var bookings := []string{}

bookings = append(bookings, 'a')
```

## Loops

```go
func printFirstNames() []string {
	firstNames := []string{}

	for index, booking := range bookings {
    fmt.Printf("index is %v\n", index)

		firstNames = append(firstNames, booking.firstName)
	}

	return firstNames
}
```

## Conditions

```go
isValidInputs := validateUserInputs(firstName, lastName, email, userTickets)

if isValidInputs {
	fmt.Printf("These are all the first names: %v\n", firstNames)
	if remainingTickets == 0 {
		fmt.Println("There no more tickets. Come back next year.")
		// break
	}
} else {
	fmt.Printf("There is only %v left. So you can't book %v \n", remainingTickets, userTickets)
}
```

## Validation
```go
func validateUserInputs(firstName string, lastName string, email string, userTickets uint) bool {
  isValidName := len(firstName) >= 2 && len(lastName) >= 2
  isValidEmail := strings.Contains(email, "@")
  isValidTickets := userTickets > 0 && userTickets <= remainingTickets

  return isValidName && isValidEmail && isValidTickets
}
```

## Packages

Create `helper.go`
```go
package helper

import "strings"

func ValidateUserInputs(firstName string, lastName string, email string, userTickets uint, remainingTickets uint) bool {
	isValidName := len(firstName) >= 2 && len(lastName) >= 2
	isValidEmail := strings.Contains(email, "@")
	isValidTickets := userTickets > 0 && userTickets <= remainingTickets

	return isValidName && isValidEmail && isValidTickets
}
```

Import it in the `main.go` file
```go
// The path matches the go.mod file.
import (
	"fmt"
	"my-go-app/helper"
)
```

## Struct

```go
type UserData struct {
	firstName    string
	lastName     string
	email        string
	ticketNumber uint
}

var bookings = make([]UserData, 0)

func bookTickets(userTickets uint, firstName string, lastName string, email string) {
	remainingTickets = remainingTickets - userTickets

	var user = UserData{
		firstName:    firstName,
		lastName:     lastName,
		email:        email,
		ticketNumber: userTickets,
	}

	bookings = append(bookings, user)

	fmt.Printf("Current bookings are %v\n", bookings)
}
```

## Concurrency

Simply add `go` before the time consuming function would create another thread.

```go
go sendTicket(userTickets, firstName, lastName, email)
```

Avoid the other threads get closed due to the  main thread closes

```go
import (
	"sync"
	"time"
)

var wg = sync.WaitGroup{}

func main() {
  wg.Add(1)
  go sendTicket(userTickets, firstName, lastName, email)

  wg.Wait()
}

func sendTicket(ticketNumber uint, firstName string, lastName string, email string) {
	time.Sleep(10 * time.Second)
	var message = fmt.Sprintf("%v tickets for %v %v\n", ticketNumber, firstName, lastName)
	fmt.Println("#########")
	fmt.Printf("message is \n %v \n email is %v\n", message, email)
	fmt.Println("#########")
	wg.Done()
}
```

Just a summary that the multi-thread in Go called goroutine. It is an abstraction of an actual thread. It is managed by the go runtime, we are only interacting with these high level goroutine. The are cheaper and lightweight comparing to the kernel level threads. Hundreds of thousands or millions goroutine can be run and not affect the program performance.

Additionally, Go has the concept of channels for supporting the communications between different threads.


## References
- [Go packages](https://pkg.go.dev/)
- [Go by example](https://gobyexample.com/)
