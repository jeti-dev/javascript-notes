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

## Arrow functions
Can't be/have:
- constructor
- super
- arguments
- `call`, `apply`, `bind`

The arrow function inside inherits `this` from the parent scope.
Another definition: `this` always refers to the object that defined the function.
``` js
function foo(){
    const hello = () => {
        console.log(this);
    }
    hello();
}

foo(); // window
foo.call({name: 'John'}); // the object
```

Can't be an object method because the object doesn't have a scope:
``` js
const user = {
    name: 'John',
    hello: ()=>console.log(this) 
}

user.hello(); // window 
```

## Object constructor function
A new object is created and `this` is set to the new object.

``` js
function Person(name) {
  this.name = name;
  this.hello = function() {
    console.log('Hello, ' + this.name);
  }
}

const person = new Person('John');

```

### Adding a property to the constructor
Use `prototype`. You can use the new property even if it was added after you created your object. Methods should be added to the prototype because this way all object will share this method resulting in less memory consumption.
``` js
function Person(name) {
  this.name = name;
  this.hello = function() {
    console.log('Hello, ' + this.name);
  }
}

const person1 = new Person('Bob');
person1.hello();

Person.prototype.goodbye = function() {
  console.log('Goodbye, ' + this.name);
};

person1.goodbye(); // Goodbye, Bob
```
### Inheritance
```js
// base class
function User(name){
    this.name = name;
}

// setting a method
User.prototype.getName = function(){
    return this.name;
}

// child class
function Admin(name, isEnabled){
    // it's like calling "super()"
    // As we don't use "new" a new object is not created. By using "this" the name property will be set on the new Admin object
    User.call(this, name);
    this.isEnabled = isEnabled;
}

// this is like "extends"
Object.setPrototypeOf(Admin.prototype, User.prototype);

const admin = new Admin('Bob', true);

console.log(admin.name);
console.log(admin.getName());
```

### With and without `new`
A function can behave differently if called with or without `new`.
``` js
Boolean(1) // true
new Boolean() // wrapper object
```

## Prototype
How objects inherit features from each other. Every object has a prototype which is also an object and has a prototype. This is the prototype chain. We reach the end of the chain when an object has a `null` prototype.

However the name of the property is not prototype. Its name is not standard but browsers usually use `__proto__`. The best way is to use `Object.getPrototypeOf()`.

When we access a property of an object and that property is not part of that object, Javascript starts tries to find that property on the object's prototype and so on.

Shadowing a property is when the child object defines a property that has a same name as a property on the prototype.

### Setting the prototype (inheritance)

`Object.create(yourProtoObject)`
``` js
const myProto = {
    hello: function() {
        return 'Hello';
    }
}

const obj = Object.create(myProto);

console.log(obj.hello()); // Hello
```

With object constructor functions we have to set the prototype of the function.
``` js
const myProto = {
    hello: function() {
        return 'Hello';
    }
}

function Person(){}

Object.assign(Person.prototype, myProto);

const person = new Person();
console.log(person.hello());
```

## Properties: `prototype` vs `__proto__`
Only functions have `prototype` and only objects have `__proto__`!
Using `__proto__` is not recommended anymore, use `Object.getPrototypeOf()` and `Object.setPrototypeOf()` instead.

`prototype` is like the blueprint for creating the objects and `__proto__` is the actual object used on the prototype chain to lookup properties.


## Classes
- Constructor
- Instance Properties
- Instance Methods
- Static Properties
- Static Methods
- Getter and Setter Methods
- Inheritance (extends)
- Method Overriding
- Private Fields (using #)
- Public Fields

``` js
// Parent class
class Animal {
  // Static property (shared across all instances)
  static totalAnimals = 0;

  // Static method (called on the class itself)
  static describe() {
    console.log("This is the Animal class.");
  }

  // Constructor (called when an instance is created)
  constructor(name, age) {
    // Instance properties
    this.name = name;
    this.age = age;

    // Private field (encapsulated, not accessible outside the class)
    this.#energy = 100;

    // Increment the static property
    Animal.totalAnimals++;
  }

  // Public field (experimental, supported in modern JS)
  habitat = "Earth";

  // Private field (encapsulated)
  #energy;

  // Getter for private field
  get energy() {
    return this.#energy;
  }

  // Setter for private field
  set energy(value) {
    if (value >= 0 && value <= 100) {
      this.#energy = value;
    } else {
      console.log("Energy must be between 0 and 100.");
    }
  }

  // Instance method
  eat(food) {
    this.#energy += 10;
    console.log(`${this.name} is eating ${food}. Energy is now ${this.#energy}.`);
  }

  // Instance method
  sleep() {
    this.#energy = 100;
    console.log(`${this.name} is sleeping. Energy is now ${this.#energy}.`);
  }

  // Overridable method
  makeSound() {
    console.log(`${this.name} makes a sound.`);
  }
}

// Child class (inherits from Animal)
class Dog extends Animal {
  constructor(name, age, breed) {
    // Call the parent constructor
    super(name, age);

    // Additional instance property
    this.breed = breed;
  }

  // Override the parent method
  makeSound() {
    console.log(`${this.name} barks!`);
  }

  // Additional instance method
  fetch(item) {
    this.energy -= 10;
    console.log(`${this.name} fetched the ${item}. Energy is now ${this.energy}.`);
  }
}

```

## IIFE (Immediately Invoked Function Expression)
``` js
(function(){
  console.log('Runs immediately')}
)()
```

## Closures
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) "A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives a function access to its outer scope."

"The lexical environment consists of any variables that were in-scope at the time the closure was created. "

"Closures are useful because they let you associate data (the lexical environment) with a function that operates on that data."

``` js

function foo(){
  var value = 'hi';

  function inner(){
    console.log(value);
  }

  inner();
}

foo(); // hi

```

"This is an example of **lexical scoping**, which describes how a parser resolves variable names when functions are nested. The word lexical refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Nested functions have access to variables declared in their outer scope."

## Functional inheritance

``` js
function createPerson(name, age) {
  // Private state (encapsulated using closures)
  let _age = age;

  // Public interface (returned object)
  return {
    name, // Public property
    getAge() {
      return _age; // Access private state
    },
    celebrateBirthday() {
      _age++; // Modify private state
      console.log(`${this.name} is now ${_age} years old!`);
    }
  };
}

const person = createPerson("Alice", 30);
console.log(person.name); // Alice (public property)
console.log(person.getAge()); // 30 (access private state)
person.celebrateBirthday(); // Alice is now 31 years old!
```

``` js
function createEmployee(name, age, jobTitle) {
  // Inherit from createPerson
  const person = createPerson(name, age);

  // Extend the object with additional properties/methods
  return {
    ...person, // Spread operator to copy properties/methods
    jobTitle,
    work() {
      console.log(`${this.name} is working as a ${this.jobTitle}.`);
    }
  };
}

const employee = createEmployee("Bob", 25, "Developer");
console.log(employee.name); // Bob (inherited from createPerson)
console.log(employee.getAge()); // 25 (inherited from createPerson)
employee.celebrateBirthday(); // Bob is now 26 years old! (inherited from createPerson)
employee.work(); // Bob is working as a Developer. (new method)
```

| Feature                | Functional Inheritance                            | Prototypal Inheritance                            |
|------------------------|--------------------------------------------------|--------------------------------------------------|
| **Encapsulation**      | Truly private state using closures.              | No native private state (until ES2022's `#`).    |
| **Memory Usage**       | Each object has its own copy of methods.         | Methods are shared via the prototype.            |
| **Flexibility**        | Highly flexible; objects can be composed dynamically. | Less flexible; relies on a fixed prototype chain. |
| **Performance**        | Slower for creating many objects.                | Faster for creating many objects.                |
| **`instanceof`**       | Cannot use `instanceof`.                         | Can use `instanceof` to check inheritance.       |