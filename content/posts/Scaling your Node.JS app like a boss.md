---
title: "Node.js To The Moon 🚀"
date: 2021-05-05T19:12:41+05:30
draft: false
---

# Scaling your Node.js app like a boss

Let's cut to the chase, you run your fancy Node.js application on your machine hoping that it'll be able to take full advantage of your CPU when you need it. But that's not what happens, let's see why and how you can make full use of the CPU power you paid for.

So a little history: JavaScript was created out of the rapidly growing demand for dynamic content on the web, it was designed to do simple things like creating a colourful mouse trail or to validate forms. It was only in 2009 that Ryan Dahl, creator of Node.js, made it possible for developers to use JavaScript to write back-end code.

Most backend languages support multithreading and have all kinds of algorithms to sync values between the threads and other thread related features. To add support for such stuff to JavaScript, Dahl needed a workaround...

## How Node.js threading works

When a Node.js process is launched, it runs:

- One process
- One thread
- One event loop
- One JS Engine Instance
- One Node.js Instance

#### One process:

A process is a global object that can be accessed anywhere and has information about what’s being executed at a time.

#### One thread:

Being single-threaded means that only one set of instructions is executed at a time in a given process.

#### One event loop:

This is what allows Node to be asynchronous and have non-blocking I/O, — despite the fact that JavaScript is single-threaded — by offloading operations to the system kernel whenever possible through callbacks, promises and async/await.

#### One JS Engine Instance:

This is a computer program that executes JavaScript code.

#### One Node.js Instance:

The computer program that executes Node.js code.

> In short, Node runs on a single thread, and there is just one process happening at a time in the event loop. Which means the code doesn't run in parallel. This is great because it solves a lot of concurrency issues. Right? 🤔

_Lol_ you wish! There's a big fat downside with this process: if you have a CPU-intensive code, like complex calculations taking place in-memory, this can block all the other code from being executed.

So if you are making a request to a server that has CPU-intensive code, that code can block the event loop and prevent other requests from being handled.

Also it doesn't even have to be just a CPU intensive code, single thread can be a bottleneck if you have a lot of incoming concurrent requests too.
