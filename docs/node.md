# Node.js

## Modules

### CommonJS

Every file is a module. Every stuff that is not exported is private to the module. The modules are wrapped in a function.

- .cjs file extension
- type: commonjs in package.json

```js
// module file
exports.foo = function(){...}
exports.bar = function(){...}

// consummer file
const mod = require("my-module");
mod.foo();
```

```js
// module file
module.exports = {
    foo: function(){...}
}

// consummer file
const mod = require("my-module");
mod.foo();

// OR
const {foo} = require("my-module");
foo();
```

This one won't work, you have to override module.exports:

```js
exports = {
  foo: function () {
    console.log("foo");
  },
  bar: function () {
    console.log("bar");
  },
};
```

### ES Modules

- .mjs extension
- type: module in package.json

| Export Method                           | Import Method                                 | Description                                                       |
| --------------------------------------- | --------------------------------------------- | ----------------------------------------------------------------- |
| `export const value = ...;`             | `import { value } from './file.js';`          | Named export, allows exporting multiple values.                   |
| `export function myFunc() {...}`        | `import { myFunc } from './file.js';`         | Named function export.                                            |
| `export class MyClass {...}`            | `import { MyClass } from './file.js';`        | Named class export.                                               |
| `export { value1, value2 };`            | `import { value1, value2 } from './file.js';` | Batch export of multiple named values.                            |
| `export default value;`                 | `import myDefault from './file.js';`          | Default export, allows importing without curly braces.            |
| `export { value as newName };`          | `import { newName } from './file.js';`        | Renaming exports.                                                 |
| `export * from './other.js';`           | `import * as all from './file.js';`           | Re-exports everything from another module.                        |
| `export { default } from './other.js';` | `import myDefault from './file.js';`          | Re-exports the default export from another module.                |
| `import './file.js';`                   | _(Runs module but does not import values)_    | Side-effect import (runs the module without storing any exports). |

### Comparison

| Feature                   | CommonJS (CJS)                                   | ES Modules (ESM)                                                   |
| ------------------------- | ------------------------------------------------ | ------------------------------------------------------------------ |
| **Syntax**                | `require()` / `module.exports`                   | `import` / `export`                                                |
| **File Extension**        | `.js` (default in Node.js)                       | `.mjs` (or `.js` with `"type": "module"`)                          |
| **Default Import/Export** | `module.exports = value;` / `require()`          | `export default value;` / `import defaultExport from './file.js';` |
| **Named Import/Export**   | `exports.key = value;` / `require('./file').key` | `export { key };` / `import { key } from './file.js';`             |
| **Import Behavior**       | **Synchronous** (Blocking)                       | **Asynchronous** (Non-blocking)                                    |
| **Execution**             | Runs immediately                                 | Runs only after parsing the module graph                           |
| **Top-Level `await`**     | ❌ Not supported                                 | ✅ Supported                                                       |
| **Tree Shaking**          | ❌ Not optimized                                 | ✅ Optimized (unused exports are removed)                          |
| **Dynamic Imports**       | ✅ Using `require()`                             | ✅ Using `import()`                                                |
| **Re-exporting**          | ❌ Not built-in                                  | ✅ `export * from './other.js';`                                   |
| **Browser Support**       | ❌ Not supported natively                        | ✅ Supported in modern browsers                                    |
| **Usage in Node.js**      | Default module system                            | Requires `"type": "module"` in `package.json` or `.mjs` extension  |
| **Best for**              | Legacy and Node.js environments                  | Modern JS applications and frontend development                    |
