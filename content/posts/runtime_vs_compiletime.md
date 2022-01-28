---
title: "Runtime_vs_compiletime"
date: 2021-10-02T17:18:54+05:30
draft: true
tags:
  - "general"
  - "programming"
  - "build time"
  - "run time"
  - "just in time"
  - "JIT"
---

# Runtime vs. Compile Time

When I started programming, I heard a lot of these words such as: "build-time", "compile time", "runtime", etc.
This really confused me after some time because the names didn't seem very intuitive to me.
So I wrote the definitions of all and put them in one place in my notebook, which is what I'm gonna be sharing in this blog 😄 .
After that we're gonna look at the difference between runtime vs compile time errors.


## Introduction

Usually, in computer programing, there are several stages of software development. These stages are required in order to convert human readable code written in a particular programming language into machine readable (or binary) code. 

## Before we start

Before we start explaining the terms. What I want you to remember is that, there are three ways to convert human readable code to machine readable code:

  1. Interpretation.
  2. Compilation.
  3. Mixture of the two.

However, what I want to establish is that the implementation is separate from the language.
The language is just a language. And the implementation is just the implementation.
Take JavaScript for example, in it's initial days, JavaScript was an interepreted language. Now, JavaScript also has a just in time compiler, the V8 engine. So the implementation of the language changed, but it's still the same language!
Hence, the implementation is seperate from the programming language and remember that moving forward.


### Compile Time

Now, Compile time refers to the time duration in which the programming code is converted to the machine code (i.e binary code) and usually occurs before runtime. During compile time, the source code is translated to a byte code.

### Build Time

The term Build is used to refer to a series of steps that put together some software deliverable. For example it usually includes compile as a single step and may then link components and previously compiled code libraries as well as bringing together other resources, assets and data into a distributable package.
Build usually includes one or more compile steps and once an application has been built it can be run.


### Runtime

Run time is simply the program's total time starting to run in the memory until the operating system terminates the program, is essentially referred as "runtime". This gernrally occurs after compile time or build time.
It was probably used in place of over using the word "when a program is run".


### Compile time vs Runtime errors


#### Compile time errors

During compile time errors occur because of syntax and semantic. The syntax error occurs because of the wrong syntax of the written code. Semantic errors occur in reference to variable, function, type declarations and type checking.

#### Runtime errors

A program’s life cycle is a runtime when the program is in execution. Following are the different types of runtime errors:

Division by zero – when a number is divided by zero (0)
Dereferencing a null pointer – when a program attempts to access memory with a NULL
Running out of memory – when a computer has no memory to allocate to programs

## Just In Time (JIT) Compilers

To understand JIT, we first need to understand the difference between Interpretation and Compilation.

### Interpretation vs Compilation

In interpretation, one instruction is read, converted to machine code and then run, and then the next instruction is read and the same cycle continues. This cycle takes time because you're going back and forth a lot, but it stops the cycle whenever an error occurs, which is good for debugging, as you know which instruction caused the error!

In compilation, all the instructions are first *compiled* together and converted to machine code and then run. This is really fast, but compilation doesn't stop until it's finished and will just move on to next instruction when an error occurs, which is harder to debug.

Hence, Interpretation is slower, but easier to debug. And Compilation is faster, but harder to debug.


So JIT Compilation is like best of both worlds, a combination of interpretation and compilation. So it is a way of executing machine readable code that involves compilation during execution of a program (runtime) rather than before execution. A system implementing a JIT compiler typically continuously analyses the code being executed and identifies parts of the code where the speedup gained from compilation or recompilation would outweigh the overhead of compiling that code. A JIT can do dynamic optimizations like eliminating whole branches of an if else statement when it knows the statement would always be true or false.  Also a JIT could optimize the code specifically for your machine, for example to avoid cache misses or to take advantage of SIMD. So although it is called Just In Time Compilation, it relies on knowing something ahead of time.