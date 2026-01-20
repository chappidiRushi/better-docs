---

id: fundamentals
title: Fundamentals
sidebar_position: 4
-------------------

# JavaScript Fundamentals

---

## What is JavaScript

JavaScript is a high-level, interpreted, single-threaded programming language used to build interactive, event-driven applications across browsers, servers, and runtimes.

Key characteristics:

* Dynamically typed
* Prototype-based object model
* Event-loop driven concurrency
* Multi-paradigm (functional, OOP, imperative)

---

## JavaScript Engine

A JavaScript Engine is a program that parses, compiles, and executes JavaScript code.

Core responsibilities:

* Parsing source code into AST
* Just-In-Time (JIT) compilation
* Memory management and garbage collection
* Execution of bytecode or optimized machine code

Popular engines:

* V8 (Chrome, Node.js)
* SpiderMonkey (Firefox)
* JavaScriptCore (Safari)

---

## Execution Context

Execution Context is the abstract environment where JavaScript code is evaluated and executed.

Each execution context contains:

* Lexical Environment (scope, bindings)
* Variable Environment (`var` declarations)
* `this` binding

Types of execution contexts:

* Global Execution Context
* Function Execution Context
* Eval Execution Context

Execution contexts are managed using the call stack and created during the creation phase before execution begins.
