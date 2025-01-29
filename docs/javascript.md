---
title: Javascript
layout: default
---

# Javascript

## Data types
### Primitives
- number
- bigint
- string
- boolean
- undefined
- null
- symbol: `Symbol()` returns a `symbol` primitive that is guaranteed to be unique in the code and which is usually used to add a unique property to an object

### Objects
- function
- array
- map
- regexp
- error: thrown when a runtime error occurs and can be used as a base class for custom errors

## Scope
- global
- module
- function
- block: only `let`, `const`, `class` and `function` (in strict mode)

Only global variables declared with `var` can be accessed through the `window` object.

## Dynamic number of function parameters
A function which can accept more parameters than what was declared is a variadic function.
### arguments
It is the "old" way and only available to non-arrow functions:
``` js
function foo(a1){
    console.log(arguments[0]); // logs 5
    console.log(arguments[1]); // logs 6
}
foo(5, 6);
```
`arguments` is an array-like object (e.g. has a `length` property) but not an array. You can convert it to an array if you need to (e.g. spread).

### Rest parameter
A function can only have one rest parameter which must be the last parameter and which can't have a default value. Your "extra" parameters will be put in your rest parameter which is an array.
``` js
function foo(a1, ...a2){
    console.log(a2); // logs [6]   
}
foo(5, 6);
```
