---
title: "Rust Terminology"
date: 2023-04-15T08:47:55-05:00
draft: false
---

# TIL

This post contains some personal notes on Rust terminology, but also learned today / this week

- Microsoft PowerApps has serious developer experience issues in Chrome on MacOS, including
  - Change a flow called by a Canvas App, and the changed input variables may or may not be propagated to the .Run command in the calling canvas app. I had to recreate the flow -- refreshing the browser did not help.
  - JSON Arrays returned from HTTP calls are difficult to work with and have a non-obvious way to interact with them
- gRPC
  -  It's like CORBA all over again!
  - Seems better suited to higher level functions than CRUD/REST type APIs and probably only shows real advantages with larger amounts of data. 
  - gRPC-Web will work over bidirectional HTTP/2 connections, but requires a proxy that seems like it will clearly limit scalability. If you think you need that you probably need something like Elixir, Phoenix, and LiveView. Or maybe F#, Fable, and Fable.Remoting -- but I suspect they have similar limitations
  - Along that vein, I think this means gRPC in the browser will be the correct choice in only a small number of situations. More likely you want some optimized reactive architecture, or SSE (server side events), etc.
  - gRPC in anything other than the browser seems like a great idea. I was able to stream events to my client app in less than an hour from when I started reading up on it.
  - It feels like incorporating gRPC into a DDD architecture where Event entities are sent over queues may not mesh very well. Not orthogonal as such; my first impression is that it just won't be needed. But I would still use it just for the IDL component to define Command and Event entities, letting the microservices be written in various languages. That's still a huge win.

# Rust Terminology Notes

Roughly 

* package = program
* crate = library (but technically can be a binary)
* module = a Pascal like module. What people use classes for these days.

When compiling, cargo looks in ./src to find a 'main.rs', which means this is a binary package, or 'lib.rs', which means this is a library package.

Putting 'mod foo' in your program means
* Search for the module inline
* If not found, try for a module in a file in the same directory. The file should be name 'foo.rs'
* If not there, try 'src/foo/mod.rs'. Note the filename changed from 'foo.rs' to 'mod.rs'

Modules can have submodules. They are searched for with the same rules as above, but instead of looking with '/src' as the root, it would use '/src/foo'. If you have a submodule bar you most likely would define it in '/src/foo/bar/mod.rs'.


