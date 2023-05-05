---
title: "Golang / PocketBase User Authentication"
date: 2023-05-05T08:25:03-05:00
draft: false
---

Recently I have been taking a side trip from Rust to the land of Go. I am producing solutions much quicker in Go. I enjoy that, but the lack of safety from the Rust compiler has bitten me twice in a week of side project work. I personally don't like the error handling patterns either but I have to admit that is only due to my use of F# and Rust. Before this year I would have lauded the Go language to the skies. Even the various concurrency patterns around goroutines are solid and elegant. For instance, discovering "for/select" was a moment of joy.

So I thought I'd note some comments of how to write a small server that talks to a [PocketBase](https://pocketbase.io) backend.

# Approach

One nice thing about Pocketbase is that you can use it as a Go package and just build on it. 
For any significant project I would argue that's the architecture to adopt. In my case I am
running it as a separate server -- at least to start -- in order to use it as a third-party API. That will let implement some of the concurrency patterns I have studied. 

Here is the code I use to access PocketBase. Pardon the clear issues; it's a learning exercise
and work in progress as well. I use the native net/http package rather than a prewritten one.

This is an implementation of a gRPC generated interface, but that's not important right now.

```go
type PocketBaseJWTAuthenticator struct {
}

// JWTAuthenticator.Authenticate
func (p PocketBaseJWTAuthenticator) Authenticate(credentials UserCredentials) (string, error) {
	// We are searching the 'users' collection in Pocketbase, not the
	// admin one.
	var urlAuthenticate = fmt.Sprintf("%s/api/collections/users/auth-with-password", pocketbaseURI)
	data := map[string]string{
		"identity": "johnstorey",
		"password": "up4down2",
	}

	jsonData, err := json.Marshal(data)
	if err != nil {
		fmt.Println(err)
		return "", err
	}

	// Make the POST request using the http.Post function.
	req, err := http.NewRequest("POST", urlAuthenticate, bytes.NewBuffer(jsonData))
	if err != nil {
		fmt.Println(err)
		return "", err
	}

	// Set the "Content-Type" header.
	req.Header.Set("Content-Type", "application/json")

	// Crete an HTTP client.
	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		fmt.Println(err)
		return "", err
	}
	defer resp.Body.Close()

	// Read the response body.
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("Error reading response body:", err)
		return "", err
	}

	bodyAsString := string(body)
	returnedUser, err := pb.CreateUserResponseFromString(bodyAsString)
	if err != nil {
		fmt.Println(err)
		return "", err
	}

	return returnedUser.Token, nil
}
```

The first thing we see is the clearness of Go code. You don't need to understand the language to
see what is happening here. I doubt the reader will have any difficulty before reaching the line

```go
	returnedUser, err := pb.CreateUserResponseFromString(bodyAsString)
```

That's beccause it's from a package I'm starting to move my Pocketbase code to a separate 
package. That call takes the HTTP response body and turns it into a Go struct. Here is the
code

```go
package pockectbase

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Record struct {
		Avatar          string `json:"avatar"`
		CollectionID    string `json:"collectionId"`
		Created         string `json:"created"`
		Email           string `json:"email"`
		EmailVisibility bool   `json:"emailVisibility"`
		ID              string `json:"id"`
		Name            string `json:"name"`
		Updated         string `json:"updated"`
		Username        string `json:"username"`
		Verified        bool   `json:"verified"`
	} `json:"record"`
	Token string `json:"token"`
}

// Create a struct from a JSON string.
func CreateUserResponseFromString(data string) (User, error) {
	var response User

	// Unmarshal the JSON data into the Response struct.
	err := json.Unmarshal([]byte(data), &response)
	if err != nil {
		fmt.Println(err)
		return User{}, err
	}

	return response, nil
}
```

Look at the nice expressiveness of the code. I can map property names in my User struct
to any JSON field I want. I have not experimented with my struct fields not matching the
JSON returned by Pocketbase. In C# that's an exception with the System.Text.Json package, but ok with the NewtonSoft.Json package. So I'll test that pretty soon. 

The only thing I see in the above code that might give the reader pause is the use of an array of bytes. 
In Go, as in Rust, you are working with the real way data is stored in memory instead of abstractions 
in the language. Since we are basically moving bytes around you end up having []byte all over your code.

# Wrap-up

That's a quick brain dump. Do not take these snippets as good examples of code architecture -- I have some time each morning between work for this project and right now am in an experimentation phase.
There are multiple code design issues I have notes on that I need to return to.

Hope this was of interest!