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

```js
function foo(a1) {
  console.log(arguments[0]); // logs 5
  console.log(arguments[1]); // logs 6
}
foo(5, 6);
```

`arguments` is an array-like object (e.g. has a `length` property) but not an array. You can convert it to an array if you need to (e.g. spread).

### Rest parameter

A function can only have one rest parameter which must be the last parameter and which can't have a default value. Your "extra" parameters will be put in your rest parameter which is an array.

```js
function foo(a1, ...a2) {
  console.log(a2); // logs [6]
}
foo(5, 6);
```

## Destructuring assignment

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

Some common use cases:

```js
const [a, b] = array;
const [a, b, ...rest] = array;
const [a = aDefault] = array;

const { a, b } = obj;
const { a: a1, b: b1 } = obj;
const { a, b, ...rest } = obj;
const { a: a1 = aDefault } = obj;

function foo({ a, b }) {
  console.log(a, b);
}

foo({
  a: 1,
  b: 2,
});
```

## Taggged templates

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)

Special syntax. Doesn't have to return a string.

```js
function myTag(strings, v1, v2) {
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

```js
const arr = [3, 5, 7];
arr.foo = "hello";

for (const i in arr) {
  console.log(i); // "0", "1", "2", "foo"
}
```

## Hoisting

"A variable can be used before it is declared."
JS moves the `var` _declarations_ to the top of the `scope`. _Initializations_ are not moved!

```js
console.log(x); // undefined, because only "var x;" is hoisted
var x = "foo";

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

```js
console.log("5" + 5); // "55" - number 5 is coerced to string
console.log("5" - 5); // 0 - string '5' is coerced to number
console.log("5" == 5); // true - string '5' is coerced to number
console.log("1" || 5); // 1 - string is coerced to boolean
```

## this

`this` refers to an object or in other words `this` refers to the context where your code runs.
Its value depends on how a function is _called_ and not how it is _defined_.

```js
const person = {
  name: "John",
  greet: function () {
    console.log("Hello, my name is " + this.name);
  },
};

//The function is called "through" the person object.
person.greet(); // Hello, my name is John

// The function is called "through" the window object that doesn't have a "name" property.
const foo = person.greet;
foo(); // Hello, my name is undefined
```

### `this` in events

It is usually the element that received the event.

### `this` in callbacks

The value of `this` depends on the API.

```js
function cb() {
  console.log(this);
}

[1, 2, 3].forEach(cb); // window, window, window
```

### `this` in arrow functions

A closure is created over the `this` value so this always refers to the object where the function was defined. This is the opposite how `this` workf for normal functions (see the intro).

```js
const cb = function () {
  console.log("cb ", this);
};

const cbArr = () => {
  console.log("cbArr ", this);
};

const cbObj = {
  cb: cb,
  cbArr: cbArr,
};

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

```js
function foo(param1, param2) {
  console.log(this.name, param1, param2);
}

foo(1, 2); // undefined 1 2

foo.call({ name: "John" }, 1, 2); // John 1 2

foo.apply({ name: "John" }, [1, 2]); // John 1 2 (the values in the array are "spread" into the function arguments)

const newFoo = foo.bind({ name: "John" }); // bind returns a new function with "this" binded to the object passed as argument
newFoo(1, 2); // John 1 2

const newFooWithParam1 = foo.bind({ name: "John" }, 1); // bind can also bind arguments
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

```js
function foo() {
  const hello = () => {
    console.log(this);
  };
  hello();
}

foo(); // window
foo.call({ name: "John" }); // the object
```

Can't be an object method because the object doesn't have a scope:

```js
const user = {
  name: "John",
  hello: () => console.log(this),
};

user.hello(); // window
```

## Object constructor function

A new object is created and `this` is set to the new object.

```js
function Person(name) {
  this.name = name;
  this.hello = function () {
    console.log("Hello, " + this.name);
  };
}

const person = new Person("John");
```

### Adding a property to the constructor

Use `prototype`. You can use the new property even if it was added after you created your object. Methods should be added to the prototype because this way all object will share this method resulting in less memory consumption.

```js
function Person(name) {
  this.name = name;
  this.hello = function () {
    console.log("Hello, " + this.name);
  };
}

const person1 = new Person("Bob");
person1.hello();

Person.prototype.goodbye = function () {
  console.log("Goodbye, " + this.name);
};

person1.goodbye(); // Goodbye, Bob
```

### Inheritance

```js
// base class
function User(name) {
  this.name = name;
}

// setting a method
User.prototype.getName = function () {
  return this.name;
};

// child class
function Admin(name, isEnabled) {
  // it's like calling "super()"
  // As we don't use "new" a new object is not created. By using "this" the name property will be set on the new Admin object
  User.call(this, name);
  this.isEnabled = isEnabled;
}

// this is like "extends"
Object.setPrototypeOf(Admin.prototype, User.prototype);

const admin = new Admin("Bob", true);

console.log(admin.name);
console.log(admin.getName());
```

### With and without `new`

A function can behave differently if called with or without `new`.

```js
Boolean(1); // true
new Boolean(); // wrapper object
```

## Prototype

How objects inherit features from each other. Every object has a prototype which is also an object and has a prototype. This is the prototype chain. We reach the end of the chain when an object has a `null` prototype.

However the name of the property is not prototype. Its name is not standard but browsers usually use `__proto__`. The best way is to use `Object.getPrototypeOf()`.

When we access a property of an object and that property is not part of that object, Javascript starts tries to find that property on the object's prototype and so on.

Shadowing a property is when the child object defines a property that has a same name as a property on the prototype.

### Setting the prototype (inheritance)

`Object.create(yourProtoObject)`

```js
const myProto = {
  hello: function () {
    return "Hello";
  },
};

const obj = Object.create(myProto);

console.log(obj.hello()); // Hello
```

With object constructor functions we have to set the prototype of the function.

```js
const myProto = {
  hello: function () {
    return "Hello";
  },
};

function Person() {}

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

```js
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
    console.log(
      `${this.name} is eating ${food}. Energy is now ${this.#energy}.`
    );
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
    console.log(
      `${this.name} fetched the ${item}. Energy is now ${this.energy}.`
    );
  }
}
```

## IIFE (Immediately Invoked Function Expression)

```js
(function () {
  console.log("Runs immediately");
})();
```

## Closures

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) "A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives a function access to its outer scope."

"The lexical environment consists of any variables that were in-scope at the time the closure was created. "

"Closures are useful because they let you associate data (the lexical environment) with a function that operates on that data."

```js
function foo() {
  var value = "hi";

  function inner() {
    console.log(value);
  }

  inner();
}

foo(); // hi
```

"This is an example of **lexical scoping**, which describes how a parser resolves variable names when functions are nested. The word lexical refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Nested functions have access to variables declared in their outer scope."

## Functional inheritance

```js
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
    },
  };
}

const person = createPerson("Alice", 30);
console.log(person.name); // Alice (public property)
console.log(person.getAge()); // 30 (access private state)
person.celebrateBirthday(); // Alice is now 31 years old!
```

```js
function createEmployee(name, age, jobTitle) {
  // Inherit from createPerson
  const person = createPerson(name, age);

  // Extend the object with additional properties/methods
  return {
    ...person, // Spread operator to copy properties/methods
    jobTitle,
    work() {
      console.log(`${this.name} is working as a ${this.jobTitle}.`);
    },
  };
}

const employee = createEmployee("Bob", 25, "Developer");
console.log(employee.name); // Bob (inherited from createPerson)
console.log(employee.getAge()); // 25 (inherited from createPerson)
employee.celebrateBirthday(); // Bob is now 26 years old! (inherited from createPerson)
employee.work(); // Bob is working as a Developer. (new method)
```

| Feature           | Functional Inheritance                                | Prototypal Inheritance                            |
| ----------------- | ----------------------------------------------------- | ------------------------------------------------- |
| **Encapsulation** | Truly private state using closures.                   | No native private state (until ES2022's `#`).     |
| **Memory Usage**  | Each object has its own copy of methods.              | Methods are shared via the prototype.             |
| **Flexibility**   | Highly flexible; objects can be composed dynamically. | Less flexible; relies on a fixed prototype chain. |
| **Performance**   | Slower for creating many objects.                     | Faster for creating many objects.                 |
| **`instanceof`**  | Cannot use `instanceof`.                              | Can use `instanceof` to check inheritance.        |

## Mixin

A set of methods that implement a feature. It can be used by other classes/objects without "extending" or "implementing" it.

```js
function User(name) {
  this.name = name;
}

const loggerMixin = {
  log: function () {
    console.log(this.name);
  },
};

Object.assign(User.prototype, loggerMixin);

const user = new User("John");
user.log();
```

## Regex

- `/ab+c/` regex lieral, can't be dynamic
- `new RegExp("ab+c")` calling the function constructor, can be dynamic

### String methods and the regex functions

| Method                           | Description                                                                                | Example                                                                  |
| -------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `match(regex)`                   | Returns an array of matches or `null` if no match is found.                                | `"hello123".match(/\d+/)` → `["123"]`                                    |
| `matchAll(regex)`                | Returns an iterator of all matches, including capturing groups.                            | `Array.from("test123test456".matchAll(/(\d+)/g))` → `[["123"], ["456"]]` |
| `search(regex)`                  | Returns the index of the first match, or `-1` if not found.                                | `"hello123".search(/\d/)` → `5`                                          |
| `replace(regex, replacement)`    | Replaces matched substrings with a replacement string or function.                         | `"hello123".replace(/\d+/, "XYZ")` → `"helloXYZ"`                        |
| `replaceAll(regex, replacement)` | Replaces all occurrences of a pattern with a replacement string or function. (**ES2021+**) | `"a1b2c3".replaceAll(/\d/g, "X")` → `"aXbXcX"`                           |
| `split(regex)`                   | Splits the string into an array based on the regex pattern.                                | `"apple,banana,grape".split(/,/)` → `["apple", "banana", "grape"]`       |
| `startsWith(str)`                | Checks if the string starts with the given substring (**not regex-based**).                | `"hello world".startsWith("hello")` → `true`                             |
| `endsWith(str)`                  | Checks if the string ends with the given substring (**not regex-based**).                  | `"hello world".endsWith("world")` → `true`                               |
| `includes(str)`                  | Checks if the string contains the given substring (**not regex-based**).                   | `"hello world".includes("lo wo")` → `true`                               |

# Regex Cheat Sheet

## Basic Syntax

| Syntax | Description                                                     |
| ------ | --------------------------------------------------------------- |
| `.`    | Matches any single character except newline (`\n`).             |
| `\d`   | Matches any digit (0-9).                                        |
| `\D`   | Matches any non-digit character.                                |
| `\w`   | Matches any word character (a-z, A-Z, 0-9, \_).                 |
| `\W`   | Matches any non-word character.                                 |
| `\s`   | Matches any whitespace character (space, tab, newline).         |
| `\S`   | Matches any non-whitespace character.                           |
| `\b`   | Matches a word boundary.                                        |
| `\B`   | Matches a non-word boundary.                                    |
| `^`    | Matches the start of a string.                                  |
| `$`    | Matches the end of a string.                                    |
| `\`    | Escapes a special character (e.g., `\.` matches a literal dot). |

## Quantifiers

| Syntax  | Description                                                       |
| ------- | ----------------------------------------------------------------- |
| `*`     | Matches 0 or more of the preceding element.                       |
| `+`     | Matches 1 or more of the preceding element.                       |
| `?`     | Matches 0 or 1 of the preceding element.                          |
| `{n}`   | Matches exactly `n` occurrences of the preceding element.         |
| `{n,}`  | Matches `n` or more occurrences of the preceding element.         |
| `{n,m}` | Matches between `n` and `m` occurrences of the preceding element. |

## Character Classes

| Syntax     | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| `[abc]`    | Matches any one of the characters `a`, `b`, or `c`.          |
| `[^abc]`   | Matches any character except `a`, `b`, or `c`.               |
| `[a-z]`    | Matches any character in the range `a` to `z`.               |
| `[A-Z]`    | Matches any character in the range `A` to `Z`.               |
| `[0-9]`    | Matches any digit in the range `0` to `9`.                   |
| `[a-zA-Z]` | Matches any character in the range `a` to `z` or `A` to `Z`. |

## Groups and Capturing

| Syntax           | Description                                                               |
| ---------------- | ------------------------------------------------------------------------- |
| `(abc)`          | Captures a group. Matches `abc` and stores it for later use.              |
| `(?:abc)`        | Non-capturing group. Matches `abc` but does not store it.                 |
| `(?<name>abc)`   | Named capturing group. Matches `abc` and stores it under the name `name`. |
| `\1`, `\2`, etc. | Backreferences to captured groups (e.g., `\1` refers to the first group). |
| `\k<name>`       | Backreference to a named capturing group.                                 |

## Disjunction (Alternation)

| Syntax | Description |
| ------ | ----------- | -------------------------- |
| `a     | b`          | Matches either `a` or `b`. |

## Lookahead and Lookbehind

| Syntax     | Description                                          |
| ---------- | ---------------------------------------------------- |
| `(?=abc)`  | Positive lookahead. Matches if `abc` is ahead.       |
| `(?!abc)`  | Negative lookahead. Matches if `abc` is not ahead.   |
| `(?<=abc)` | Positive lookbehind. Matches if `abc` is behind.     |
| `(?<!abc)` | Negative lookbehind. Matches if `abc` is not behind. |

## Flags

| Flag | Description                                                                                             | Example                                                               |
| ---- | ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `g`  | **Global search**: Finds all matches in the string, not just the first one.                             | `"hello hello".match(/hello/g)` → `["hello", "hello"]`                |
| `i`  | **Case-insensitive search**: Ignores case when matching.                                                | `"Hello".match(/hello/i)` → `["Hello"]`                               |
| `m`  | **Multiline mode**: `^` and `$` match the start/end of each line, not just the start/end of the string. | `"line1\nline2".match(/^line2/m)` → `["line2"]`                       |
| `s`  | **Dot-all mode**: Allows `.` to match newline characters (`\n`).                                        | `"line1\nline2".match(/line1.line2/s)` → `["line1\nline2"]`           |
| `u`  | **Unicode mode**: Treats the pattern as a sequence of Unicode code points.                              | `"😊".match(/\u{1F60A}/u)` → `["😊"]`                                 |
| `y`  | **Sticky mode**: Matches only from the index indicated by `lastIndex`.                                  | `const regex = /a/y; regex.lastIndex = 1; regex.test("baa")` → `true` |

## Examples

| Example                                          | Description                                                              |
| ------------------------------------------------ | ------------------------------------------------------------------------ |
| `/hello/`                                        | Matches the exact string `"hello"`.                                      |
| `/^hello/`                                       | Matches `"hello"` at the start of a string.                              |
| `/hello$/`                                       | Matches `"hello"` at the end of a string.                                |
| `/h.llo/`                                        | Matches `"hallo"`, `"hello"`, `"hxllo"`, etc.                            |
| `/h\*llo/`                                       | Matches `"hllo"`, `"hello"`, `"heello"`, etc.                            |
| `/h\+llo/`                                       | Matches `"hello"`, `"heello"`, etc. (at least one `e`).                  |
| `/he?llo/`                                       | Matches `"hllo"` or `"hello"`.                                           |
| `/he{2}llo/`                                     | Matches `"heello"`.                                                      |
| `/he{2,4}llo/`                                   | Matches `"heello"`, `"heeello"`, or `"heeeello"`.                        |
| `/[aeiou]/`                                      | Matches any vowel.                                                       |
| `/[^aeiou]/`                                     | Matches any non-vowel.                                                   |
| `/(\w+)\s\1/`                                    | Matches repeated words (e.g., `"hello hello"`).                          |
| `/(?<=@)\w+/`                                    | Matches a username in an email (e.g., `"user"` in `"user@example.com"`). |
| `/hello/gi`                                      | Matches all occurrences of `"hello"` case-insensitively.                 |
| `/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/` | Named capturing groups for date format (e.g., `2023-10-05`).             |
| `/cat\|dog/`                                     | Matches either `"cat"` or `"dog"`.                                       |

|

## `typeof`

| Return Value  | Description                                                            | Example                                                                            |
| ------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `"undefined"` | The value is `undefined`.                                              | `typeof undefined` → `"undefined"`                                                 |
| `"boolean"`   | The value is a boolean (`true` or `false`).                            | `typeof true` → `"boolean"`                                                        |
| `"number"`    | The value is a number (including `NaN` and `Infinity`).                | `typeof 42` → `"number"`                                                           |
| `"string"`    | The value is a string.                                                 | `typeof "hello"` → `"string"`                                                      |
| `"bigint"`    | The value is a `BigInt`.                                               | `typeof 123n` → `"bigint"`                                                         |
| `"symbol"`    | The value is a `Symbol`.                                               | `typeof Symbol("foo")` → `"symbol"`                                                |
| `"function"`  | The value is a function (or a class, which is technically a function). | `typeof function() {}` → `"function"`                                              |
| `"object"`    | The value is an object (including `null`, arrays, and objects).        | `typeof {}` → `"object"`<br>`typeof null` → `"object"`<br>`typeof []` → `"object"` |

## Symbol

A `symbol` is a unique value usually used to create object keys.

```js
const s = Symbol(); // using new throws an error!
const sd = Symbol("description"); // just a description
```

You can create a `symbol` which is unique acrosss files and "realms". The next method creates a `symbol` for the given key or returns an existing `symbol` for the key from the registry:

```js
const sk = Symbol.for("key");
const key = Symbol.keyFor(sk); // get the key of the symbol
```

`Symbol` properties of objects are not iterable (they won't appear in `for ... in`) and they are ignored by `JSON.stringify()` too!

### Well-known Symbol

These static properties of `Symbol` can be used to "implement" some behaviours.

| Symbol                      | Description                                                                         | Example                                                                                                                                                                                     |
| --------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Symbol.iterator`           | Used to define the default iterator for an object.                                  | `const obj = { [Symbol.iterator]: function* () { yield 1; yield 2; } }; [...obj]` → `[1, 2]`                                                                                                |
| `Symbol.asyncIterator`      | Used to define the default async iterator for an object.                            | `const obj = { [Symbol.asyncIterator]: async function* () { yield 1; yield 2; } }; (async () => { for await (const val of obj) console.log(val); })();` → `1, 2`                            |
| `Symbol.match`              | Used to customize the behavior of `String.prototype.match`.                         | `const obj = { [Symbol.match]: () => "custom" }; "hello".match(obj)` → `"custom"`                                                                                                           |
| `Symbol.replace`            | Used to customize the behavior of `String.prototype.replace`.                       | `const obj = { [Symbol.replace]: () => "world" }; "hello".replace(obj)` → `"world"`                                                                                                         |
| `Symbol.search`             | Used to customize the behavior of `String.prototype.search`.                        | `const obj = { [Symbol.search]: () => 2 }; "hello".search(obj)` → `2`                                                                                                                       |
| `Symbol.split`              | Used to customize the behavior of `String.prototype.split`.                         | `const obj = { [Symbol.split]: () => ["he", "llo"] }; "hello".split(obj)` → `["he", "llo"]`                                                                                                 |
| `Symbol.hasInstance`        | Used to customize the behavior of `instanceof`.                                     | `class MyClass { static [Symbol.hasInstance]() { return true; } }; console.log({} instanceof MyClass);` → `true`                                                                            |
| `Symbol.isConcatSpreadable` | Used to determine if an object should be flattened during `Array.prototype.concat`. | `const obj = { [Symbol.isConcatSpreadable]: true, length: 2, 0: 3, 1: 4 }; [1, 2].concat(obj)` → `[1, 2, 3, 4]`                                                                             |
| `Symbol.unscopables`        | Used to exclude properties from being bound by `with` statements.                   | `Array.prototype[Symbol.unscopables]` → `{ copyWithin: true, entries: true, ... }`                                                                                                          |
| `Symbol.species`            | Used to customize the constructor used for derived objects.                         | `class MyArray extends Array { static get [Symbol.species]() { return Array; } }; const myArray = new MyArray(1, 2, 3); console.log(myArray.map(x => x * 2) instanceof MyArray);` → `false` |
| `Symbol.toPrimitive`        | Used to customize the behavior of object-to-primitive conversion.                   | `const obj = { [Symbol.toPrimitive]: (hint) => hint === "number" ? 42 : "forty-two" }; console.log(obj + 1);` → `"forty-two1"`                                                              |
| `Symbol.toStringTag`        | Used to customize the default description of an object.                             | `const obj = { [Symbol.toStringTag]: "MyObject" }; console.log(obj.toString());` → `"[object MyObject]"`                                                                                    |

## Set, Map, WeakSet, WeakMap

### Set

Unique values of primitives and object references. You can iterate over the items in the order they were added to the `Set`. Quite fast.

[Set composition visually](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#set_composition)

`Set` methods work with set-like objects:

- has a `size` property
- has a `keys()` method that returns an iterator
- has a `has()` method

| Method                                       | Description                                                                                             |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `Set.prototype.add(value)`                   | Adds a new element with the given value to the set. Returns the set itself.                             |
| `Set.prototype.delete(value)`                | Removes the specified element from the set. Returns `true` if the value was removed, otherwise `false`. |
| `Set.prototype.has(value)`                   | Returns `true` if the value exists in the set, otherwise `false`.                                       |
| `Set.prototype.clear()`                      | Removes all elements from the set.                                                                      |
| `Set.prototype.values()`                     | Returns a new iterator object containing all values in the set.                                         |
| `Set.prototype.keys()`                       | Alias for `values()`, since `Set` does not have keys.                                                   |
| `Set.prototype.entries()`                    | Returns a new iterator object containing `[value, value]` pairs (to align with `Map`).                  |
| `Set.prototype.forEach(callbackFn, thisArg)` | Calls `callbackFn` once for each value in the set, in insertion order.                                  |

| Method                                        | Description                                                                           |
| --------------------------------------------- | ------------------------------------------------------------------------------------- |
| `Set.prototype.intersection(otherSet)`        | Returns a new set containing elements common to both sets.                            |
| `Set.prototype.union(otherSet)`               | Returns a new set containing all elements from both sets.                             |
| `Set.prototype.difference(otherSet)`          | Returns a new set containing elements present in the first set but not in the second. |
| `Set.prototype.symmetricDifference(otherSet)` | Returns a new set with elements present in either set but not in both.                |
| `Set.prototype.isSubsetOf(otherSet)`          | Returns `true` if all elements of the first set exist in the second set.              |
| `Set.prototype.isSupersetOf(otherSet)`        | Returns `true` if the first set contains all elements of the second set.              |

### Map

Key-value pairs but remembers the order of how they keys were added. A key can be any value.
Map-like objects:

- read-only: size, entries(), forEach(), get(), has(), keys(), values(), and Symbol.iterator
- writeable additionally has: clear(), delete(), and set()

| Method                                       | Description                                                                                    |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `Map.prototype.set(key, value)`              | Adds or updates a key-value pair in the map. Returns the map itself.                           |
| `Map.prototype.get(key)`                     | Returns the value associated with the given key, or `undefined` if the key is not found.       |
| `Map.prototype.has(key)`                     | Returns `true` if the map contains the specified key, otherwise `false`.                       |
| `Map.prototype.delete(key)`                  | Removes the key-value pair from the map. Returns `true` if the key existed, otherwise `false`. |
| `Map.prototype.clear()`                      | Removes all key-value pairs from the map.                                                      |
| `Map.prototype.size`                         | Returns the number of key-value pairs in the map. (Not a function, but a property.)            |
| `Map.prototype.keys()`                       | Returns a new iterator object containing all keys in the map.                                  |
| `Map.prototype.values()`                     | Returns a new iterator object containing all values in the map.                                |
| `Map.prototype.entries()`                    | Returns a new iterator object containing `[key, value]` pairs for each entry in the map.       |
| `Map.prototype.forEach(callbackFn, thisArg)` | Calls `callbackFn` once for each key-value pair in insertion order.                            |

### WeakSet

Only objects and non-registered symbols can be stored. If the elements in the `WeakSet` are not referenced elsewhere they can be deleted by the garbage collector. Usage: e.g. recursive function where there are multiple identical objects so you can "tag" an object if that has been visited before. This is why it only has these methods:
| Method | Description |
|------------------------|-------------|
| `WeakSet.prototype.add(value)` | Adds an object to the `WeakSet`. Returns the `WeakSet` itself. |
| `WeakSet.prototype.delete(value)` | Removes the given object from the `WeakSet`. Returns `true` if the object was found and removed, otherwise `false`. |
| `WeakSet.prototype.has(value)` | Returns `true` if the object exists in the `WeakSet`, otherwise `false`. |

Use case: On click you want to do something with a DOM node but do it only for the first time. You can put them in a `WeakSet` so you can check if you've seen that node already and you don't prevent it from being garbage collected.

### WeakMap

The keys can be only objects or non-registered symbols. When a key is garbage collacted then its value is also garbage collected.

| Method                              | Description                                                                                          |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `WeakMap.prototype.set(key, value)` | Adds a key-value pair to the `WeakMap`. The key must be an object.                                   |
| `WeakMap.prototype.get(key)`        | Returns the value associated with the given object key, or `undefined` if the key is not found.      |
| `WeakMap.prototype.has(key)`        | Returns `true` if the key exists in the `WeakMap`, otherwise `false`.                                |
| `WeakMap.prototype.delete(key)`     | Removes the key-value pair from the `WeakMap`. Returns `true` if the key existed, otherwise `false`. |

When the DOM element is deleted, the number is deleted too:

```js
const hoverCounts = new WeakMap();

function trackHover(event) {
  let element = event.target;

  // Get the current count or initialize it to 0
  let count = hoverCounts.get(element) || 0;
  hoverCounts.set(element, count + 1);

  console.log(`Hovered ${hoverCounts.get(element)} times`);
}

// Attach event listeners
document.querySelectorAll(".track-hover").forEach((element) => {
  element.addEventListener("mouseenter", trackHover);
});
```

## `try...catch`

- `finally` runs every time. In the example above it log "Finally" first then the return value 1.
- If we returned a value from `finally` that would be the return value of the function (it even overrides a `throw` in the `catch`!) but it is a bad practice.
- The exception usually has a name and a message property.
- An exception is only caught once by the closest `catch` block.
- optional catch binding: You don't need to declare the param in the `catch` block.
- `new Error('msg')`: `msg` will be the `e.message`.
- You can `throw` any value.
- You can extend the `Error` class to create custom errors.

```js
function foo() {
  try {
    return 1;
  } catch (e) {
    console.log(e.name, e.message);
  } finally {
    console.log("Finally");
  }
}

console.log(foo());
```

You can't "filter" the exception type like in other languages but you can do e.g.:

```js
try {
  foo();
} catch (e) {
  if (e instanceof RangeError) {
    // handle only this type of error
  } else {
    throw e; // re-throw the error
  }
}
```

Again, `finally` runs every time! Logs "finally" then "catch err".

```js
try {
  try {
    throw new Error("err");
  } finally {
    console.log("finally"); // logged first, no catch is here!
  }
} catch (e) {
  console.error("catch", e.message); // logged second
}
```

## Promise

3 states:

- pending
- fulfilled
- rejected
- (settled: after fulfilled or rejected)

Functions:

- `then(successHandler, errorHandler)`
- `catch(errorHandler)`
- `finally(handler)`

A promise cna be added to more than 1 chain:

```js
const a = new Promise(myExecutorFunc);
const b = a.then(h1); // runs first
const c = a.then(h2); // runs second
```

`thenable` is something that has a `then(successHandler, errorHandler)` method.

### Concurrency

| Method                         | Description                                                                                                            | Example                                                                    |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `Promise.resolve(value)`       | Returns a promise that resolves with the given value. If the value is a promise, it is returned as-is.                 | `Promise.resolve(42).then(console.log)` → `42`                             |
| `Promise.reject(reason)`       | Returns a promise that rejects with the given reason.                                                                  | `Promise.reject("Error!").catch(console.error)` → `Error!`                 |
| `Promise.all(iterable)`        | Runs multiple promises in parallel and resolves when all fulfill, or rejects if any fail.                              | `Promise.all([p1, p2])` → Resolves when all are fulfilled.                 |
| `Promise.allSettled(iterable)` | Runs multiple promises in parallel and resolves when all have settled (fulfilled or rejected).                         | `Promise.allSettled([p1, p2])` → Array of `{status, value/reason}`         |
| `Promise.race(iterable)`       | Runs multiple promises and settles as soon as the first one settles (fulfilled or rejected).                           | `Promise.race([p1, p2])` → Settles with the first resolved/rejected value. |
| `Promise.any(iterable)`        | Runs multiple promises and resolves with the first one that fulfills. If all reject, it rejects with `AggregateError`. | `Promise.any([p1, p2])` → Resolves with the first fulfilled value.         |

### Promisification

```js
function promisify(originalFunctionWithCallback) {
  return function (...args) {
    // collect all parameters in args
    return new Promise((resolve, reject) => {
      function callback(err, result) {
        // util callback for originalFunctionUsingCallback -> it will be called by that function
        if (err) {
          reject(err);
        } else {
          resolve(result);
        }
      }

      args.push(callback); // put the callback at the end of args

      originalFunctionWithCallback.call(this, ...args); // call the original function, our callback will be the last param which is usually where it is expected
    });
  };
}

function getData(params, callback) {
  console.log(`getData: ${params}`);
  setTimeout(() => {
    const res = { name: "Joe" };
    console.log(`returning: ${res}`);
    callback(null, res);
  }, 1000);
}

// usage:
let getDataPromisified = promisify(getData);
getDataPromisified("name").then((e) => console.log("result: ", e));
```

| Aspect                | `.then(success, failure)`                                         | `.then(success).catch(failure)`                             |
| --------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------- |
| **Error Scope**       | Only handles errors in the **current** `.then()` block.           | Handles **all errors** in the chain up to that point.       |
| **Error Propagation** | **Breaks** the chain if an error occurs.                          | **Preserves** the chain for further processing.             |
| **Chaining Behavior** | Stops error propagation, next `.then()` is not executed on error. | Continues the chain after error handling.                   |
| **Readability**       | Less readable for complex chains.                                 | Easier to read and maintain for consistent error handling.  |
| **Common Use Case**   | Simple operations where error propagation isn't needed.           | Complex chains where consistent error handling is required. |
| **Best Practice**     | Not recommended for chained promises.                             | Recommended for chaining with consistent error handling.    |
| **Example**           | `promise.then(success, failure)`                                  | `promise.then(success).catch(failure)`                      |

## Iterator

An object has to implement the `Symbol.iterator` method to be iterable.

```js
const numbers = {};

// Make it Iterable
numbers[Symbol.iterator] = function () {
  let n = 0;
  done = false;
  return {
    next() {
      n += 10;
      if (n == 100) {
        done = true;
      }
      return { value: n, done: done };
    },
  };
};

for (const num of numbers) {
  // will run until done = false
}
```

## Generators

All generators are iterators but not all iterators all generators.
Syntax:

```js
function* myGenerator() {
  // implement
  yield 1;
}

const generator = myGenerator();
generator().next().value; // 1
```

- Use Iterators for manual control and performance-critical tasks.
- Use Generators for complex logic, lazy evaluation, and infinite sequences.

[Source](https://stackoverflow.com/questions/37124006/iterator-and-a-generator-in-javascript)

```js
function* mygen() {
  // 1st next(): return 12, myVal is undefined
  // 2nd next(): myVal gets the value passed to next()
  var myVal = yield 12;
  return myVal * 2;
}

const myIt = mygen();

const firstGenValue = myIt.next().value; // 12
const result = myIt.next(firstGenValue * 2).value;

console.log(result); // 48
```

Async generator [Source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function*#using_an_async_generator_function_to_read_a_series_of_files)

```js
async function* readFiles(directory) {
  const files = await fs.readdir(directory);
  for (const file of files) {
    const stats = await fs.stat(file);
    if (stats.isFile()) {
      yield {
        name: file,
        content: await fs.readFile(file, "utf8"),
      };
    }
  }
}

const files = readFiles(".");
console.log((await files.next()).value);
// Possible output: { name: 'file1.txt', content: '...' }
console.log((await files.next()).value);
// Possible output: { name: 'file2.txt', content: '...' }
```
