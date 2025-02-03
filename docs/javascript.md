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
## Destructuring assignment
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

Some common use cases:
``` js
const [a, b] = array;
const [a, b, ...rest] = array;
const [a = aDefault] = array;

const { a, b } = obj;
const { a: a1, b: b1 } = obj;
const { a, b, ...rest } = obj;
const { a: a1 = aDefault } = obj;

function foo({a, b}){
    console.log(a, b)
}

foo({
    a: 1,
    b: 2
});
```

## Taggged templates
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)

Special syntax. Doesn't have to return a string.

``` js
function myTag(strings, v1, v2){
    return strings[0] + v1 + strings[1] + v2 + strings[2];
}

const msg = myTag`Hello ${"World"} this is ${"JavaScript"}`;

console.log(msg); // Hello World this is JavaScript
```

## Loops
### for...of
Iterates over the values of an iterable object:
- Array
- String
- TypedArray
- Map
- Set
- NodeList (and other DOM collections) 
- object
- generators

### for...in
Iterates over the enumerable string properties of an object (ignores symbol properties).
It might yield suprising results:
``` js
const arr = [3, 5, 7];
arr.foo = "hello";

for (const i in arr) {
  console.log(i); // "0", "1", "2", "foo"
}

```
## Hoisting
"A variable can be used before it is declared."
JS moves the `var` *declarations* to the top of the `scope`. *Initializations* are not moved!
``` js

console.log(x); // undefined, because only "var x;" is hoisted
var x = 'foo';


y = 2;
console.log(y); // 2, because "var y;" is moved before "y = 2;"
var y;

```

## Temporal dead zone (TDZ)
A variable declared with `let`, `const` or `class` is in the temporal dead zone from the start of the block until the execution reaches their declaration. In other words you can't use them before they are declared.

## Type coercion
[Link](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/)
- to string: `+`
- to boolean: `|| && !`
- to number: `> < <= >= | & ^ ~ - + * / % +(unary) == !=`

A few example:
``` js
console.log('5' + 5); // "55" - number 5 is coerced to string
console.log('5' - 5); // 0 - string '5' is coerced to number
console.log('5' == 5); // true - string '5' is coerced to number
console.log('1' || 5); // 1 - string is coerced to boolean
```

## this
`this` refers to an object or in other words `this` refers to the context where your code runs.
Its value depends on how a function is *called* and not how it is *defined*.

``` js
const person = {
    name: 'John',
    greet: function() {
        console.log('Hello, my name is ' + this.name);
    }
}

//The function is called "through" the person object.
person.greet() // Hello, my name is John

// The function is called "through" the window object that doesn't have a "name" property.
const foo = person.greet;
foo() // Hello, my name is undefined
```

### `this` in events
It is usually the element that received the event.

### `this` in callbacks
The value of `this` depends on the API.
``` js
function cb(){
    console.log(this);
}

[1, 2, 3].forEach(cb); // window, window, window
```

### `this` in arrow functions
A closure is created over the `this` value so this always refers to the object where the function was defined. This is the opposite how `this` workf for normal functions (see the intro).
```js
const cb = function() {
    console.log('cb ', this);
}

const cbArr = () =>{
    console.log('cbArr ',this);
}

const cbObj = {
    cb: cb,
    cbArr: cbArr
}

cbObj.cb(); // cbObj
cbObj.cbArr(); // window
```

### `this` in constructors
`this` refers to the new object being constructed.

### `call`, `apply`, `bind`
All of the functions bind `this` to an object in some way.
- `call`: call the function with the provided object as `this` and provide the parameters one by one
- `apply`: call the function with the provided object as `this` and provide the parameters in an array which is then spread to the parameters
- `bind`: create a new function with the provided object as `this`

``` js
function foo(param1, param2){
    console.log(this.name, param1, param2);
}

foo(1, 2); // undefined 1 2

foo.call({name: 'John'}, 1, 2); // John 1 2

foo.apply({name: 'John'}, [1, 2]); // John 1 2 (the values in the array are "spread" into the function arguments)

const newFoo = foo.bind({name: 'John'}); // bind returns a new function with "this" binded to the object passed as argument 
newFoo(1, 2); // John 1 2

const newFooWithParam1 = foo.bind({name: 'John'}, 1); // bind can also bind arguments
newFooWithParam1(2); // John 1 2
```