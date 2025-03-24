---
title: Typescript
layout: default
---

- any vs unknown: You can't assign an unkown value to any other typed variable and you can't use methods an an unkown before casting.
- void vs never return type: void is when we don't return anything so undefined is returned automatically. never is when the function never finishes or throws an error and that automatic return of undefined doesn't happen.
- type vs interface: type can be used as an alias for other types and can "contain" unions. An interface can use declaration merging that means if the interface is defined 2 times with different properties then all of the properties will be peresent in the interface.
- keyof: Returns a string or number union of the keys of an object. `type Point = { x: number; y: number }; type P = keyof Point;` it's the same as `type P = "x" | "y"`
- typeof: Get the type of a value. E.g. the "signature" of a function.
- discriminating unions: `type Animal = Dog | Cat` and both have a common property `species`. If you `switch` or `if-else` `species` then TS lets you use the  Cat or Dog props in that block.


## Mapped types
You want to reuse the prop names but want to change the types: `type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};` it creates a new type which has all they keys from the generic but all of them are boolean. You can also change if the properties are optional or readonly.


| Utility Type | Description | Example |
|-------------|-------------|---------|
| `Partial<T>` | Makes all properties of `T` optional | `Partial<User>` |
| `Required<T>` | Makes all properties of `T` required | `Required<OptionalUser>` |
| `Readonly<T>` | Makes all properties of `T` readonly | `Readonly<User>` |
| `Pick<T, K>` | Creates a type with a subset of keys from `T` | `Pick<User, 'id' | 'name'>` |
| `Record<K, T>` | Creates a type with a set of keys and a specific value type | `Record<'id' | 'name', string>` |

## Temaplate literal types
Replaces the variables in the string with all the possible values.

``` ts
type HttpMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';
type ApiEndpoint = `/api/${string}/${HttpMethod}`;

const endpoint: ApiEndpoint = "/api/user/GET"; // ✅ Valid
const invalidEndpoint: ApiEndpoint = "/api/user/UPDATE"; // ❌ Error
```
##  Utility Types 

| Utility Type | Description | Example |
|-------------|-------------|---------|
| **Partial\<T>** | Makes all properties of type `T` optional. | `type User = Partial<{ name: string; age: number }>;` |
| **Required\<T>** | Makes all properties of type `T` required. | `type User = Required<{ name?: string; age?: number }>;` |
| **Readonly\<T>** | Makes all properties of type `T` read-only. | `type User = Readonly<{ name: string; age: number }>;` |
| **Record<K, T>** | Creates an object type with keys `K` and values of type `T`. | `type UserRoles = Record<'admin' | 'user', boolean>;` |
| **Pick<T, K>** | Creates a type by picking a set of properties `K` from `T`. | `type User = Pick<{ name: string; age: number }, 'name'>;` |
| **Omit<T, K>** | Creates a type by omitting a set of properties `K` from `T`. | `type User = Omit<{ name: string; age: number }, 'age'>;` |
| **Exclude<T, U>** | Removes types from `T` that are assignable to `U`. | `type Status = Exclude<'success' | 'error' | 'pending', 'pending'>;` |
| **Extract<T, U>** | Extracts types from `T` that are assignable to `U`. | `type Status = Extract<'success' | 'error' | 'pending', 'success' | 'error'>;` |
| **NonNullable<T>** | Removes `null` and `undefined` from `T`. | `type Status = NonNullable<string | null | undefined>;` |
| **ReturnType<T>** | Extracts the return type of a function type. | `type Output = ReturnType<() => string>;` |
| **InstanceType<T>** | Extracts the instance type of a constructor function type. | `type MyClass = InstanceType<typeof MyClass>;` |
| **ConstructorParameters<T>** | Extracts the types of the parameters of a constructor. | `type Params = ConstructorParameters<typeof MyClass>;` |
| **Parameters<T>** | Extracts the types of the parameters of a function type. | `type Params = Parameters<(a: string, b: number) => void>;` |
| **ThisParameterType<T>** | Extracts the `this` parameter type of a function type. | `type T = ThisParameterType<(this: Date, x: number) => void>;` |
| **OmitThisParameter<T>** | Removes the `this` parameter from a function type. | `type T = OmitThisParameter<(this: Date, x: number) => void>;` |
| **Uppercase<T>** | Converts string literal types to uppercase. | `type T = Uppercase<'hello'>; // 'HELLO'` |
| **Lowercase<T>** | Converts string literal types to lowercase. | `type T = Lowercase<'HELLO'>; // 'hello'` |
| **Capitalize<T>** | Converts the first character of a string literal type to uppercase. | `type T = Capitalize<'hello'>; // 'Hello'` |
| **Uncapitalize<T>** | Converts the first character of a string literal type to lowercase. | `type T = Uncapitalize<'Hello'>; // 'hello'` |

## Decorators
Decorator types: class, method, property, parameter, accessor.
``` ts
// Method decorator example
function logMethod(target: any, key: string, descriptor: PropertyDescriptor) {
  console.log(`Method: ${key}`);
}

class MyClass {
  @logMethod
  myMethod() {}
}
```

``` ts
// Decorator factory
function myDecorator() {
  console.log("factory evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("called");
  };
}
 
```

## Type guard
`pet if Fish` - when this guard is called, TS will know if in the block we have a fish or a bird.

``` ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

let pet = getSmallPet();
 
if (isFish(pet)) {
  pet.swim();
} else {
  pet.fly();
}
```

## satisfies
The values of the object can be an array of a string. `satisfies` ensures correct types but it is also aware of the types so it knows that green is indeed a string.

``` ts
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];
const palette = {
    red: [255, 0, 0],
    green: "#00ff00",
    blue: [0, 0, 255]
//  ~~~~ The typo is now caught!
} satisfies Record<Colors, string | RGB>;
// toUpperCase() method is still accessible!
const greenNormalized = palette.green.toUpperCase();
```

## Config

| Property | Description | Example |
|----------|-------------|---------|
| **path aliases** | Allows creating short import paths using the `paths` option under `compilerOptions`. | `"paths": { "@utils/*": ["src/utils/*"] }` |
| **extends** | Inherits configurations from another `tsconfig.json` file. Useful for sharing configs across projects. | `"extends": "./base-tsconfig.json"` |
| **lib** | Specifies a list of library files to include in the compilation. | `"lib": ["ESNext", "DOM"]` |
| **noImplicitAny** | Throws an error if a variable's type is not explicitly defined. | `"noImplicitAny": true` |
| **strict** | Enables all strict type-checking options. | `"strict": true` |
| **strictNullChecks** | Forces explicit handling of `null` and `undefined` types. | `"strictNullChecks": true` |
| **noImplicitOverride** | Ensures that a method marked as `override` exists in the parent class. | `"noImplicitOverride": true` |
| **noImplicitReturns** | Ensures that all code paths in a function return a value. | `"noImplicitReturns": true` |
| **noImplicitThis** | Reports an error if `this` is implicitly of type `any`. | `"noImplicitThis": true` |
| **strictPropertyInitialization** | Ensures that class properties are initialized in the constructor or marked as optional. | `"strictPropertyInitialization": true` |
| **useUnknownInCatch** | Makes the `catch` clause variable of type `unknown` instead of `any`. | `"useUnknownInCatch": true` |
| **downLevelIteration** | Enables full support for iteration in `for...of` loops in ES5/ES3. | `"downlevelIteration": true"` |
| **target** | Specifies the ECMAScript version to compile to. | `"target": "ESNext"` |
| **module** | Specifies the module system to use (`CommonJS`, `ESNext`, etc.). | `"module": "ESNext"` |
| **outDir** | Specifies the output directory for compiled files. | `"outDir": "./dist"` |
| **rootDir** | Specifies the root directory for TypeScript files. | `"rootDir": "./src"` |
| **moduleResolution** | Determines how modules are resolved (`node`, `classic`). | `"moduleResolution": "node"` |
| **sourceMap** | Generates `.map` files for easier debugging. | `"sourceMap": true` |
| **declaration** | Generates `.d.ts` declaration files for type definitions. | `"declaration": true` |
| **skipLibCheck** | Skips type-checking of declaration files. | `"skipLibCheck": true` |
| **removeComments** | Removes comments in the compiled output. | `"removeComments": true` |
| **jsx** | Specifies how JSX syntax is compiled. | `"jsx": "react"` |
| **baseUrl** | Specifies the base directory for resolving non-relative imports. | `"baseUrl": "./src"` |
| **esModuleInterop** | Enables compatibility with `import` syntax in CommonJS modules. | `"esModuleInterop": true` |
| **experimentalDecorators** | Enables experimental support for decorators. | `"experimentalDecorators": true` |
| **emitDecoratorMetadata** | Emits metadata for decorators at runtime. | `"emitDecoratorMetadata": true` |
| **isolatedModules** | Ensures that each file is treated as a separate module. | `"isolatedModules": true` |

## Compiler optimization

| **Optimization Technique** | **Description** | **Example** |
|---------------------------|----------------|-------------|
| **Skip Lib Check** | Skips type checking of declaration files (`.d.ts`) to speed up the build process. | `"skipLibCheck": true` |
| **Incremental Compilation** | Reuses information from previous compilations to speed up the next one. | `"incremental": true` |
| **Isolated Modules** | Ensures each file can be transpiled separately, improving incremental builds and compatibility with Babel. | `"isolatedModules": true` |
| **No Emit** | Compiles the code without generating output files, useful for type-checking only. | `"noEmit": true` |
| **Emit Declaration Only** | Only generates `.d.ts` files without producing `.js` files, useful for library builds. | `"emitDeclarationOnly": true` |
