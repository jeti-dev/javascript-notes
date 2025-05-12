---
title: Node
layout: default
---

# Node.js

## Process vs thread

A process is an instance of a running program with its own memory space and execution environment.

- In Node.js, each script runs in a single process by default.
- Processes do not share memory with each other.
- The Cluster module is used to spawn multiple processes.

✅ Use Processes (Cluster/Child Processes) when:

- You need to run separate applications or services.
- Your tasks are independent and don’t need shared memory.
- You want load balancing (e.g., multiple workers handling requests).

A thread is a lightweight unit of execution within a process.

- Threads share memory with their parent process.
- Node.js is single-threaded by default but uses worker threads for parallel execution.

✅ Use Threads (Worker Threads) when:

- You need to perform CPU-intensive tasks in parallel (e.g., data processing).
- You require shared memory access between tasks.
- You want to offload heavy computations without blocking the event loop.

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
| **Top-Level `await`**     | ❌ Not supported                                 | ✅ Supported (async call at the top makes the importing modules wait)                                                       |
| **Tree Shaking**          | ❌ Not optimized                                 | ✅ Optimized (unused exports are removed)                          |
| **Dynamic Imports**       | ✅ Using `require()`                             | ✅ Using `import()`                                                |
| **Re-exporting**          | ❌ Not built-in                                  | ✅ `export * from './other.js';`                                   |
| **Browser Support**       | ❌ Not supported natively                        | ✅ Supported in modern browsers                                    |
| **Usage in Node.js**      | Default module system                            | Requires `"type": "module"` in `package.json` or `.mjs` extension  |
| **Best for**              | Legacy and Node.js environments                  | Modern JS applications and frontend development                    |

## Event driven architecture

| **Advantage**                    | **Explanation**                                                                                                     | **Disadvantage**                      | **Explanation**                                                                                          |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Asynchronous & Non-Blocking**  | The event loop allows I/O operations to run without blocking execution, improving performance.                      | **Harder Debugging & Error Handling** | Since execution is non-linear, errors are harder to trace and may be swallowed if not handled properly.  |
| **Efficient for I/O Operations** | Ideal for applications handling multiple network requests, file I/O, or database queries simultaneously.            | **Callback Hell (Deep Nesting)**      | Heavy reliance on callbacks can lead to deeply nested code, making it hard to read and maintain.         |
| **Scalable & Lightweight**       | Uses fewer system resources than multi-threaded architectures, making it well-suited for high-traffic applications. | **Single-Threaded Bottlenecks**       | CPU-intensive tasks block the event loop, slowing down all operations and making the app unresponsive.   |
| **Real-Time Capabilities**       | Well-suited for real-time applications like chat apps, online gaming, and live notifications.                       | **Potential Memory Leaks**            | If event listeners aren’t removed properly, memory consumption increases, leading to performance issues. |
| **Decoupled Components**         | Components communicate via events, allowing for better modularity and separation of concerns.                       | **Complex Code Structure**            | Large applications may become difficult to manage without a well-structured event-driven architecture.   |

## Async timing

Event loop:

There are 6 phases of the event loop, but in practice we need to use 4.
Each of these 4 phases have their **own task queue** where the callbacks are stored.

- **timers**: callbacks scheduled by setTimeout() or setInterval()
- **pending callbacks**: I/O callbacks (e.g. file read, webserver)
- idle, prepare: internal
- poll: retrieve new I/O events
- **check**: setImmediate() callbacks
- **close callbacks**: some close callbacks, e.g. socket.on('close', fn).

- promises and `process.nextTick()` callbacks have their own queues (micro task queues) which have priority over the macro task queues (nextTick comes first then promise callbacks)

- When you call a function, it is put on the call stack.
- When you call an asynchronous function, it is not put on the call stack. It just registers a callback function.
- When the async function is ready, the callback function is put on the appropriate task queue.
- When there are no more functions on the call stack, the event loop starts checking the 4 task queues for tasks. If there are tasks in a given queue, it starts putting those tasks on the stack and executes them.
- When there are no more tasks in a queue, it moves to the next type of queue.
- BUT: The microstask quesues are prioritized over the macro task queues. So the event loop always starts with checking the microtask queues and if there are no tasks, it checks the timer queue which is the first queue in the list of the 5 queues. After each macro task queue it checks again the microtask queues because the macrotask queue might have added something to the microtask queue!

Example:

- check microtask
- check timers
- check microtask
- check pending callbacks
- ...

![Event loop](/assets/eventloop_node.png)
[Source](https://www.builder.io/blog/visual-guide-to-nodejs-event-loop)

`queueMicrotask`
[Source](https://nodejs.org/api/process.html#when-to-use-queuemicrotask-vs-processnexttick)

- uses the same queue as promises
- in ESM modules `queueMicrotask()` runs before `process.nextTick` callbacks!
- it uses the same queue as promises so the order depends on which was added first!

| **Method**           | **Execution Timing**                                       | **Order in Event Loop**                                            | **Use Case**                                                                 |
| -------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| `process.nextTick()` | Before I/O, timers, and `setImmediate()`                   | **Highest Priority** (executes immediately after synchronous code) | Critical tasks that must execute before moving to the next event loop cycle. |
| `queueMicrotask()`   | Before next event loop phase, after resolved Promises      | **After `process.nextTick()` but before I/O**                      | Short async tasks that shouldn't block the event loop.                       |
| `setImmediate()`     | After I/O callbacks, before next event loop cycle          | **Runs after I/O callbacks**                                       | Tasks that should run after I/O but before the next loop iteration.          |
| `setTimeout(fn, 0)`  | After at least the specified delay (0 ms means next cycle) | **Runs after timers phase**                                        | Delayed execution, runs in the **timers phase** after I/O.                   |

```js
const fs = require("fs");

console.log("Start");

fs.readFile("pg.html", () => {
  console.log("IO callback");
});

setTimeout(() => {
  console.log("Timeout callback");

  process.nextTick(() => console.log("Next Tick inside Timeout"));
}, 0);

setImmediate(() => {
  console.log("Immediate callback");
});

Promise.resolve().then(() => {
  console.log("Promise callback");
});

process.nextTick(() => {
  console.log("Next Tick callback");
});

console.log("End");

// Start
// End
// Next Tick callback         microtask prio 1
// Promise callback           microtask prio 2
// Timeout callback           macrotask 1
// Next Tick inside Timeout   microtask created in the timeout queue!
// Immediate callback         microtask 3
// IO callback                tricky one, it was triggered in the second event loop cycle because it took more time! (?)
```

## Console

| **Method**                  | **Description**                                    | **Example**                                 |
| --------------------------- | -------------------------------------------------- | ------------------------------------------- |
| `console.log()`             | Logs general messages.                             | `console.log("Hello, World!");`             |
| `console.error()`           | Logs error messages to `stderr`.                   | `console.error("Error occurred!");`         |
| `console.warn()`            | Logs warning messages.                             | `console.warn("This is a warning!");`       |
| `console.info()`            | Alias for `console.log()`, used for info logs.     | `console.info("Information message");`      |
| `console.debug()`           | Alias for `console.log()`, used for debugging.     | `console.debug("Debug details");`           |
| `console.table(data)`       | Displays tabular data.                             | `console.table([{name: "John", age: 30}]);` |
| `console.group(label)`      | Starts a collapsible group.                        | `console.group("Fruits");`                  |
| `console.groupCollapsed()`  | Starts a collapsed group.                          | `console.groupCollapsed("Hidden");`         |
| `console.groupEnd()`        | Ends the last opened group.                        | `console.groupEnd();`                       |
| `console.time(label)`       | Starts a timer.                                    | `console.time("myTimer");`                  |
| `console.timeEnd(label)`    | Ends a timer and logs the elapsed time.            | `console.timeEnd("myTimer");`               |
| `console.count(label)`      | Logs how many times a label was counted.           | `console.count("Apples");`                  |
| `console.countReset(label)` | Resets the count for a label.                      | `console.countReset("Apples");`             |
| `console.assert(cond, msg)` | Logs a message only if the condition is **false**. | `console.assert(1 === 2, "Oops!");`         |
| `console.trace(label)`      | Prints a stack trace.                              | `console.trace("Debugging trace");`         |
| `console.clear()`           | Clears the console output.                         | `console.clear();`                          |

## File system

| **Method**                        | **Description**                                         | **Example**                                        |
| --------------------------------- | ------------------------------------------------------- | -------------------------------------------------- |
| `fs.readFile(path, callback)`     | Reads a file asynchronously with a callback.            | `fs.readFile("file.txt", cb);`                     |
| `fs.writeFile(path, data, cb)`    | Writes data to a file asynchronously.                   | `fs.writeFile("file.txt", "Hello", cb);`           |
| `fs.appendFile(path, data, cb)`   | Appends data to a file asynchronously.                  | `fs.appendFile("file.txt", "More", cb);`           |
| `fs.rename(oldPath, newPath, cb)` | Renames a file asynchronously.                          | `fs.rename("old.txt", "new.txt", cb);`             |
| `fs.unlink(path, cb)`             | Deletes a file asynchronously.                          | `fs.unlink("file.txt", cb);`                       |
| `fs.stat(path, cb)`               | Gets file statistics (size, type, etc.) asynchronously. | `fs.stat("file.txt", cb);`                         |
| `fs.mkdir(path, cb)`              | Creates a directory asynchronously.                     | `fs.mkdir("myDir", cb);`                           |
| `fs.rmdir(path, cb)`              | Removes a directory asynchronously.                     | `fs.rmdir("myDir", cb);`                           |
| `fs.readdir(path, cb)`            | Reads the contents of a directory asynchronously.       | `fs.readdir("myDir", cb);`                         |
| `fs.access(path, cb)`             | Checks if a file is accessible with permissions.        | `fs.access("file.txt", cb);`                       |
| `fs.copyFile(src, dest, cb)`      | Copies a file asynchronously.                           | `fs.copyFile("src.txt", "dest.txt", cb);`          |
| `fs.createReadStream(path)`       | Creates a readable file stream.                         | `const stream = fs.createReadStream("file.txt");`  |
| `fs.createWriteStream(path)`      | Creates a writable file stream.                         | `const stream = fs.createWriteStream("file.txt");` |

| **Promise-based Method**               | **Description**                         | **Example**                                          |
| -------------------------------------- | --------------------------------------- | ---------------------------------------------------- |
| `fs.promises.readFile(path)`           | Reads a file as a promise.              | `await fs.promises.readFile("file.txt");`            |
| `fs.promises.writeFile(path, data)`    | Writes data to a file as a promise.     | `await fs.promises.writeFile("file.txt", "Hello");`  |
| `fs.promises.appendFile(path, data)`   | Appends data to a file as a promise.    | `await fs.promises.appendFile("file.txt", "More");`  |
| `fs.promises.rename(oldPath, newPath)` | Renames a file as a promise.            | `await fs.promises.rename("old.txt", "new.txt");`    |
| `fs.promises.unlink(path)`             | Deletes a file as a promise.            | `await fs.promises.unlink("file.txt");`              |
| `fs.promises.stat(path)`               | Gets file statistics as a promise.      | `await fs.promises.stat("file.txt");`                |
| `fs.promises.mkdir(path)`              | Creates a directory as a promise.       | `await fs.promises.mkdir("myDir");`                  |
| `fs.promises.rmdir(path)`              | Removes a directory as a promise.       | `await fs.promises.rmdir("myDir");`                  |
| `fs.promises.readdir(path)`            | Reads directory contents as a promise.  | `await fs.promises.readdir("myDir");`                |
| `fs.promises.access(path)`             | Checks file accessibility as a promise. | `await fs.promises.access("file.txt");`              |
| `fs.promises.copyFile(src, dest)`      | Copies a file as a promise.             | `await fs.promises.copyFile("src.txt", "dest.txt");` |

### Readable stream

```js
const fs = require("fs");

const stream = fs.createReadStream("large-log.txt", "utf8");

stream.on("data", (chunk) => {
  console.log("Received chunk:", chunk);
});

stream.on("end", () => {
  console.log("Finished reading file.");
});

stream.on("error", (err) => {
  console.error("Error reading file:", err);
});
```

### Writable stream

```js
const fs = require("fs");

const stream = fs.createWriteStream("server-log.txt", { flags: "a" });

stream.write("Server started at " + new Date().toISOString() + "\n");
stream.write("New user connected.\n");

stream.end();
```

### Async generators

```js
const fs = require("fs");
const readline = require("readline");

async function* readLines(filePath) {
  const fileStream = fs.createReadStream(filePath, "utf8");
  const rl = readline.createInterface({ input: fileStream });

  for await (const line of rl) {
    yield line;
  }
}

// Usage
(async () => {
  for await (const line of readLines("large-log.txt")) {
    console.log("Line:", line);
  }
})();
```

```js
const fs = require("fs");
const readline = require("readline");

async function* readJSONLines(filePath) {
  const fileStream = fs.createReadStream(filePath, "utf8");
  const rl = readline.createInterface({ input: fileStream });

  for await (const line of rl) {
    yield JSON.parse(line);
  }
}

// Usage
(async () => {
  for await (const obj of readJSONLines("large-data.jsonl")) {
    console.log("JSON Object:", obj);
  }
})();
```

```js
const http = require("http");
const fs = require("fs");

async function* streamFile(filePath) {
  const stream = fs.createReadStream(filePath, "utf8");
  for await (const chunk of stream) {
    yield chunk;
  }
}

http
  .createServer(async (req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });

    for await (const chunk of streamFile("large-file.txt")) {
      res.write(chunk);
    }

    res.end();
  })
  .listen(3000, () => console.log("Server running on port 3000"));
```

```js
const fs = require("fs");
const readline = require("readline");

async function* processCSV(filePath) {
  const stream = fs.createReadStream(filePath, "utf8");
  const rl = readline.createInterface({ input: stream });

  const headers = (await rl[Symbol.asyncIterator]().next()).value.split(",");

  for await (const line of rl) {
    const values = line.split(",");
    const json = Object.fromEntries(headers.map((h, i) => [h, values[i]]));
    yield JSON.stringify(json) + "\n";
  }
}

async function writeJSON(outputPath, generator) {
  const writeStream = fs.createWriteStream(outputPath, { flags: "w" });

  for await (const json of generator) {
    writeStream.write(json);
  }

  writeStream.end();
}

// Convert "data.csv" to "output.json"
writeJSON("output.json", processCSV("data.csv")).then(() =>
  console.log("CSV transformed to JSON")
);
```

```js
const fs = require("fs");
const fetch = require("node-fetch");

async function* fetchPaginatedData(apiUrl) {
  let page = 1;

  while (true) {
    const res = await fetch(`${apiUrl}?page=${page}`);
    const data = await res.json();

    if (data.length === 0) break; // Stop when no more data

    yield JSON.stringify(data) + "\n";
    page++;
  }
}

async function saveToFile(outputFile, generator) {
  const stream = fs.createWriteStream(outputFile, { flags: "w" });

  for await (const chunk of generator) {
    stream.write(chunk);
  }

  stream.end();
}

// Fetch and store API data
saveToFile(
  "api-data.json",
  fetchPaginatedData("https://jsonplaceholder.typicode.com/posts")
).then(() => console.log("API data saved."));
```

## StringDecoder

The `string_decoder` module is used to decode buffers into strings without losing multi-byte characters.  
It is ideal for streamed or chunked data processing where characters may be split across chunks.

| Method                        | Description                                                                  |
| ----------------------------- | ---------------------------------------------------------------------------- |
| `new StringDecoder(encoding)` | Creates a new decoder for a specified encoding (default: `"utf8"`).          |
| `decoder.write(buffer)`       | Decodes a buffer chunk into a string while preserving multi-byte characters. |
| `decoder.end([buffer])`       | Flushes remaining data in the buffer and clears internal state.              |

```js
const { StringDecoder } = require("string_decoder");
const decoder = new StringDecoder("utf8");

const buf1 = Buffer.from([0xe2, 0x82]); // Incomplete character
const buf2 = Buffer.from([0xac]); // Remaining part

console.log(decoder.write(buf1)); // ""
console.log(decoder.write(buf2)); // "€"
```

## Timer

Node.js timers work within the event loop and are executed asynchronously. The execution timing depends on:

- Timer Phase in the event loop (setTimeout and setInterval run in the Timers Phase).
- I/O Callbacks and Event Queue (setImmediate() runs after I/O callbacks).
- Microtasks like process.nextTick() run before the event loop continues.

The Timer API provides functions for scheduling **delayed** and **recurring** execution of code.

| Method                                      | Description                                                  | Execution Timing                    |
| ------------------------------------------- | ------------------------------------------------------------ | ----------------------------------- |
| **`setTimeout(callback, delay, ...args)`**  | Runs `callback` once after `delay` milliseconds.             | Timers Phase (after delay)          |
| **`clearTimeout(timeoutId)`**               | Cancels a `setTimeout()` timer.                              | Immediately                         |
| **`setInterval(callback, delay, ...args)`** | Runs `callback` repeatedly every `delay` milliseconds.       | Timers Phase (every delay)          |
| **`clearInterval(intervalId)`**             | Stops a `setInterval()` timer.                               | Immediately                         |
| **`setImmediate(callback, ...args)`**       | Runs `callback` on the next event loop iteration, after I/O. | Check Phase                         |
| **`clearImmediate(immediateId)`**           | Cancels a `setImmediate()` timer.                            | Immediately                         |
| **`process.nextTick(callback, ...args)`**   | Runs `callback` before the next event loop cycle.            | Before next phase (Microtask Queue) |

## URI

| Method/Property                           | Description                                                                                    |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `encodeURI(uri)`                          | Encodes a URI by escaping special characters (except `:, /, ? & = + $ #`).                     |
| `encodeURIComponent(uriComponent)`        | Encodes a URI component, escaping all special characters, making it safe for query parameters. |
| `decodeURI(encodedURI)`                   | Decodes an encoded URI back to its original format.                                            |
| `decodeURIComponent(encodedURIComponent)` | Decodes an encoded URI component.                                                              |
| `URL`                                     | The constructor for creating and parsing URLs.                                                 |
| `URLSearchParams`                         | An API to work with query string parameters easily.                                            |
| `URL.href`                                | The full URL as a string.                                                                      |
| `URL.protocol`                            | Gets or sets the protocol (`http:`, `https:`, etc.).                                           |
| `URL.host`                                | The hostname and port (`example.com:3000`).                                                    |
| `URL.hostname`                            | The domain name (`example.com`).                                                               |
| `URL.port`                                | The port number (`3000`).                                                                      |
| `URL.pathname`                            | The path of the URL (`/some/page`).                                                            |
| `URL.search`                              | The query string (`?key=value`).                                                               |
| `URL.hash`                                | The fragment (`#section`).                                                                     |
| `URLSearchParams.get(name)`               | Retrieves the value of a query parameter.                                                      |
| `URLSearchParams.set(name, value)`        | Updates or adds a query parameter.                                                             |
| `URLSearchParams.append(name, value)`     | Appends a new query parameter without replacing existing ones.                                 |
| `URLSearchParams.delete(name)`            | Removes a query parameter.                                                                     |
| `URLSearchParams.toString()`              | Converts query parameters to a string format (`key=value&another=value`).                      |

## Buffer

Buffers allow you to store, manipulate, and transfer binary data easily.
Not really used directly but it used under the hood for streams and other stuff. E.g. `readFileSync` can return a `Buffer` and create a base64 strin.

| Method/Property               | Description                                                                                            | Example                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| `Buffer.alloc(size)`          | Creates a new `Buffer` of the specified size, filled with zeros.                                       | `const buf = Buffer.alloc(10);` → `Buffer of size 10`                       |
| `Buffer.allocUnsafe(size)`    | Creates a new `Buffer` of the specified size, but does not initialize it.                              | `const buf = Buffer.allocUnsafe(10);` → `Uninitialized Buffer of size 10`   |
| `Buffer.from(array)`          | Creates a `Buffer` from an array of bytes.                                                             | `const buf = Buffer.from([1, 2, 3]);` → `Buffer with values [1, 2, 3]`      |
| `Buffer.from(string)`         | Creates a `Buffer` from a string.                                                                      | `const buf = Buffer.from("hello");` → `Buffer with ASCII values of "hello"` |
| `Buffer.from(buffer)`         | Creates a copy of an existing `Buffer`.                                                                | `const buf2 = Buffer.from(buf);` → `Copy of buf`                            |
| `Buffer.byteLength(string)`   | Returns the number of bytes required to store a string.                                                | `Buffer.byteLength("hello");` → `5`                                         |
| `Buffer.concat(list)`         | Concatenates an array of `Buffer` objects into a single `Buffer`.                                      | `const buf = Buffer.concat([buf1, buf2]);` → `Concatenated Buffer`          |
| `Buffer.isBuffer(obj)`        | Checks if an object is a `Buffer`.                                                                     | `Buffer.isBuffer(buf);` → `true`                                            |
| `buf.length`                  | Returns the size of the `Buffer` in bytes.                                                             | `buf.length;` → `10`                                                        |
| `buf.toString([encoding])`    | Converts the `Buffer` to a string using the specified encoding (default: `utf8`).                      | `buf.toString("utf8");` → `"hello"`                                         |
| `buf.write(string, [offset])` | Writes a string to the `Buffer` at the specified offset.                                               | `buf.write("world", 0);` → `Buffer with "world"`                            |
| `buf.slice([start, end])`     | Returns a new `Buffer` that references the same memory as the original, but with a subset of the data. | `const slice = buf.slice(0, 5);` → `Buffer with first 5 bytes`              |
| `buf.copy(target, [start])`   | Copies data from the `Buffer` to a target `Buffer`.                                                    | `buf.copy(targetBuf, 0);` → `Copies data to targetBuf`                      |
| `buf.equals(otherBuffer)`     | Checks if two `Buffer` objects have the same bytes.                                                    | `buf.equals(buf2);` → `true` or `false`                                     |
| `buf.indexOf(value)`          | Returns the index of the first occurrence of a value in the `Buffer`.                                  | `buf.indexOf("h");` → `0`                                                   |
| `buf.includes(value)`         | Checks if the `Buffer` contains a value.                                                               | `buf.includes("hello");` → `true`                                           |
| `buf.fill(value)`             | Fills the `Buffer` with the specified value.                                                           | `buf.fill(0);` → `Buffer filled with zeros`                                 |

## Cluster

Run multiple instances of your app to distribute workload.
However `worker_threads` should be used when process isolation is not needed. `worker_threads` allows running multiple application threads within a single Node.js instance.
Cluster is built on top of `child_process.fork` and `cluster.fork` just enables to listen to a shared port.
IPC = inter process communication.

| Method / Property                                | Description                                                     | Best Use Case                                                             |
| ------------------------------------------------ | --------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `cluster.isMaster` (Deprecated: Use `isPrimary`) | Boolean indicating if the process is the master/primary process | Running different logic for master and worker processes                   |
| `cluster.isWorker`                               | Boolean indicating if the process is a worker                   | Distinguishing worker processes from the master process                   |
| `cluster.fork()`                                 | Creates a new worker process                                    | Scaling Node.js applications across CPU cores                             |
| `cluster.on('exit', callback)`                   | Detects when a worker process exits                             | Restarting crashed workers for reliability                                |
| `cluster.on('online', callback)`                 | Fires when a worker process is successfully spawned             | Logging successful worker creation                                        |
| `cluster.on('message', callback)`                | Listens for messages from workers                               | Implementing IPC (Inter-Process Communication) between master and workers |
| `worker.send(message)`                           | Sends a message from the master to a worker                     | Passing data/tasks to worker processes                                    |
| `worker.process`                                 | Accesses the underlying `child_process` object                  | Managing worker details like PIDs                                         |
| `worker.id`                                      | Unique identifier of the worker process                         | Tracking workers for debugging or load balancing                          |
| `worker.kill()`                                  | Terminates a worker process                                     | Stopping a specific worker when needed                                    |
| `cluster.setupMaster(options)`                   | Configures how workers are spawned                              | Customizing worker forking behavior                                       |
| `Object.keys(cluster.workers)`                   | Gets an array of all worker IDs                                 | Managing multiple worker processes dynamically                            |
| `cluster.workers[id]`                            | Gets a specific worker by its ID                                | Sending targeted messages to specific workers                             |

```js
const cluster = require("cluster");
const http = require("http");
const os = require("os");

if (cluster.isMaster) {
  const numCPUs = os.cpus().length; // Get the number of CPU cores

  console.log(`Master process ${process.pid} is running`);

  // Fork workers (one per CPU core)
  for (let i = 0; i < numCPUs; i++) {
    const worker = cluster.fork();

    // Listen for messages from workers - IPC from Child to Parent: receive message
    worker.on("message", (message) => {
      console.log(
        `Master received message from Worker ${worker.process.pid}:`,
        message
      );
    });

    // IPC - Master to Child: send message
    setTimeout(() => {
      worker.send({ msg: "Hello from master" });
    }, 2000);
  }

  // Restart a worker if one dies
  cluster.on("exit", (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died. Starting a new one...`);
    cluster.fork();
  });
} else {
  // IPC - Master to Child: receive message
  process.on("message", (message) => {
    console.log(`Worker ${process.pid} received message from master:`, message);
  });

  // Worker process: Create an HTTP server
  http
    .createServer((req, res) => {
      res.writeHead(200);
      res.end(`Handled by worker ${process.pid}\n`);

      // Worker sends a message to the master - IPC from Child to Parent: send message
      process.send({ pid: process.pid, status: "Request Handled" });
    })
    .listen(3000);

  console.log(`Worker ${process.pid} started`);
}
```

PROs

- ✅ High-traffic web servers – Allows Node.js to handle more concurrent requests.
- ✅ CPU-intensive tasks – Spawns multiple processes to distribute the workload.
- ✅ Microservices & APIs – Helps in scaling backend services efficiently.
- ✅ WebSockets & Real-time Apps – Improves handling of multiple connections.

CONs

- 🚨 Not Shared Memory – Workers do not share memory, so you need inter-process communication (IPC).
- 🚨 Not for Heavy Computation – For CPU-heavy tasks, consider worker threads instead.
- 🚨 State Management Issues – Sessions or caches should be stored in a database (e.g., Redis) instead of worker memory.

## child_process

```js
const express = require("express");
const { fork } = require("child_process");

const app = express();

app.get("/compute", (req, res) => {
  const child = fork("child.js"); // Fork a new process

  child.send(5e9); // Send input to child process

  child.on("message", (result) => {
    res.send(`Computed result: ${result}`);
  });

  child.on("error", (err) => {
    res.status(500).send(`Error in child process: ${err.message}`);
  });
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

```js
process.on("message", (n) => {
  let sum = 0;
  for (let i = 0; i < n; i++) {
    sum += i;
  }
  process.send(sum); // Send result back to parent
});
```

| Method                              | Description                                           | Best Use Case                                          |
| ----------------------------------- | ----------------------------------------------------- | ------------------------------------------------------ |
| `exec(command, callback)`           | Runs a shell command and buffers output               | Running simple shell commands like `ls`, `git pull`    |
| `execFile(file, args, callback)`    | Runs an executable file without a shell               | Running scripts or binaries efficiently                |
| `spawn(command, args, options)`     | Spawns a new process and streams data                 | Handling large output (e.g., `ping`, `ffmpeg`)         |
| `fork(modulePath, args, options)`   | Spawns a Node.js child process with IPC               | Running scripts with inter-process communication (IPC) |
| `execSync(command)`                 | Runs a shell command synchronously and returns output | Short synchronous tasks where blocking is acceptable   |
| `spawnSync(command, args, options)` | Synchronously spawns a new process                    | Running a command and waiting for the result           |
| `child.send(message)`               | Sends a message to a child process                    | Communicating between parent and child processes       |
| `child.on('message', callback)`     | Listens for messages from a child process             | Handling responses from a child process                |
| `child.kill(signal)`                | Terminates a child process                            | Gracefully stopping long-running tasks                 |
| `child.stdout.on('data', callback)` | Listens for stdout output from a process              | Capturing real-time output from a command              |
| `child.stderr.on('data', callback)` | Listens for error output from a process               | Capturing errors from a command                        |

## worker threads

| Method / Property                    | Description                                              | Best Use Case                                              |
| ------------------------------------ | -------------------------------------------------------- | ---------------------------------------------------------- |
| `new Worker(filename, options)`      | Creates a new worker thread                              | Running CPU-intensive tasks in parallel                    |
| `worker.postMessage(value)`          | Sends a message to the worker thread                     | Passing data from the main thread to the worker            |
| `worker.on('message', callback)`     | Listens for messages from the worker                     | Receiving results from the worker thread                   |
| `worker.terminate()`                 | Stops the worker thread                                  | Gracefully shutting down workers when done                 |
| `parentPort.postMessage(value)`      | Sends a message from a worker to the main thread         | Communicating results back to the parent                   |
| `parentPort.on('message', callback)` | Listens for messages from the main thread                | Receiving tasks/data in a worker                           |
| `isMainThread`                       | Boolean indicating if the script runs in the main thread | Conditionally executing main-thread or worker-thread logic |
| `workerData`                         | Data passed to the worker at startup                     | Sending immutable startup data to a worker                 |
| `worker.threadId`                    | Unique identifier of the worker thread                   | Debugging and tracking workers                             |
| `worker.resourceLimits`              | Object containing memory and CPU limits                  | Setting constraints to prevent resource overuse            |
| `MessageChannel`                     | Creates a channel for communication between threads      | Efficient inter-worker communication                       |
| `SharedArrayBuffer`                  | Shared memory buffer across threads                      | High-performance data sharing without copying              |
| `Atomics`                            | Ensures safe concurrent memory operations                | Synchronizing access to shared memory (js tuff not node)                      |

### Shared memory

```js
const { Worker, isMainThread, workerData } = require("worker_threads");

if (isMainThread) {
  const sharedBuffer = new SharedArrayBuffer(4); // 4 bytes for an Int32
  const counter = new Int32Array(sharedBuffer); // Shared Int32 array

  console.log(`Main thread: Initial counter value: ${counter[0]}`);

  for (let i = 0; i < 3; i++) {
    new Worker(__filename, { workerData: sharedBuffer });
  }
} else {
  const counter = new Int32Array(workerData);

  for (let i = 0; i < 1000; i++) {
    Atomics.add(counter, 0, 1); // Safely increment shared counter
  }

  console.log(`Worker thread: Counter updated to ${Atomics.load(counter, 0)}`);
}
```

## Crypto

- hash: A one-way transformation, cannot be reverted.
- encryption: Two-way transformation.
- cipher: An encrypting and decrypthing algorithm.
- iv: Initialization vector - adds randomness to the start of the encrypted file, must be unique for each encrytion.

- hash: An attacker could modify the file and re-create the hash too.
- HMAC: Uses also a key for the hashing so the attacker must know the key too.

- Use hashing for password storage (sha256, bcrypt, PBKDF2).
- Use HMAC for secure authentication.
- Use AES for symmetric encryption (encrypt/decrypt with same key).
- Use RSA/ECDSA for public/private key encryption.
- Use digital signatures to verify authenticity.
- Use randomBytes for tokens and security.

| Method                                                                | Description                                     | Use Case                                           |
| --------------------------------------------------------------------- | ----------------------------------------------- | -------------------------------------------------- |
| `crypto.createHash(algorithm)`                                        | Creates a hash function                         | Storing passwords securely (one-way encryption)    |
| `crypto.createHmac(algorithm, key)`                                   | Creates a keyed-hash function (HMAC)            | API authentication, message integrity              |
| `crypto.createCipheriv(algorithm, key, iv)`                           | Creates a cipher for symmetric encryption       | Encrypting sensitive data like tokens              |
| `crypto.createDecipheriv(algorithm, key, iv)`                         | Creates a decipher to decrypt data              | Decrypting data encrypted with `createCipheriv`    |
| `crypto.generateKeyPairSync(type, options)`                           | Generates asymmetric RSA/ECDSA key pairs        | Public-private key encryption                      |
| `crypto.publicEncrypt(key, data)`                                     | Encrypts data using a public key                | Securely transmitting data                         |
| `crypto.privateDecrypt(key, encryptedData)`                           | Decrypts data using a private key               | Decrypting received encrypted messages             |
| `crypto.createSign(algorithm)`                                        | Creates a digital signature                     | Signing documents or transactions                  |
| `crypto.createVerify(algorithm)`                                      | Verifies a digital signature                    | Ensuring message authenticity                      |
| `crypto.pbkdf2(password, salt, iterations, keylen, digest, callback)` | Derives a key from a password (PBKDF2)          | Securely hashing passwords                         |
| `crypto.randomBytes(size)`                                            | Generates cryptographically strong random bytes | Creating secure tokens and session IDs             |
| `crypto.randomUUID()`                                                 | Generates a random UUID (v4)                    | Unique identifiers for objects, users, or requests |

## NET

The `net` module provides networking capabilities for TCP servers and clients.

| Feature            | Description                                                     |
| ------------------ | --------------------------------------------------------------- |
| **TCP Server**     | Create a TCP server with `net.createServer()`.                  |
| **TCP Client**     | Connect to a TCP server using `net.Socket()`.                   |
| **Full-Duplex**    | Enables both reading and writing at the same time.              |
| **Event-Driven**   | Uses events like `connection`, `data`, `error`, `close`.        |
| **Stream Support** | Works with Node.js streams for handling large data efficiently. |

```js
const net = require("net");

const server = net.createServer((socket) => {
  console.log("Client connected");

  socket.on("data", (data) => {
    console.log("Received:", data.toString());
    socket.write("Message received");
  });

  socket.on("end", () => console.log("Client disconnected"));
});

server.listen(3000, () => console.log("Server running on port 3000"));
```

## OS

The `os` module provides useful system-related information like CPU, memory, and network details.

| Method                   | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `os.platform()`          | Returns the OS platform (`win32`, `linux`, `darwin`).        |
| `os.version()`           | Returns the OS version.                                      |
| `os.arch()`              | Returns the CPU architecture (`x64`, `arm`, `ia32`).         |
| `os.type()`              | Returns the OS name (`Windows_NT`, `Linux`, `Darwin`).       |
| `os.release()`           | Returns the OS release version.                              |
| `os.uptime()`            | Returns system uptime in seconds.                            |
| `os.totalmem()`          | Returns total system memory (RAM) in bytes.                  |
| `os.freemem()`           | Returns available system memory (RAM) in bytes.              |
| `os.cpus()`              | Returns an array of CPU core details (model, speed, times).  |
| `os.homedir()`           | Returns the current user’s home directory.                   |
| `os.tmpdir()`            | Returns the OS's temporary directory path.                   |
| `os.userInfo()`          | Returns an object with details about the current user.       |
| `os.hostname()`          | Returns the hostname of the operating system.                |
| `os.networkInterfaces()` | Returns details of available network interfaces.             |
| `os.endianness()`        | Returns the CPU endianness (`LE` or `BE`).                   |
| `os.EOL`                 | Returns the system's end-of-line character (`\n` or `\r\n`). |

## ReadLine

The `readline` module provides an interface for reading user input and processing text streams line-by-line.

| Method                              | Description                                              |
| ----------------------------------- | -------------------------------------------------------- |
| `readline.createInterface(options)` | Creates a readline interface with input/output streams.  |
| `rl.question(query, callback)`      | Asks a question and passes the response to the callback. |
| `rl.on("line", callback)`           | Fires when a new line is received from input.            |
| `rl.on("close", callback)`          | Fires when the interface is closed.                      |
| `rl.on("pause", callback)`          | Fires when input is paused.                              |
| `rl.on("resume", callback)`         | Fires when input resumes after being paused.             |
| `rl.write(data)`                    | Writes text to the output stream.                        |
| `rl.pause()`                        | Temporarily stops accepting input.                       |
| `rl.resume()`                       | Resumes input after pausing.                             |
| `rl.close()`                        | Closes the readline interface.                           |

```js
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.question("Enter your name: ", (name) => {
  console.log(`Hello, ${name}!`);
  rl.close();
});
```

## Stream

Streams allow handling **large amounts of data efficiently** by processing it in chunks.

| Type          | Description                          | Example                               |
| ------------- | ------------------------------------ | ------------------------------------- |
| **Readable**  | Data flows _into_ the program.       | File reading (`fs.createReadStream`)  |
| **Writable**  | Data flows _out of_ the program.     | File writing (`fs.createWriteStream`) |
| **Duplex**    | Can both read and write.             | Network sockets (`net.Socket`)        |
| **Transform** | Modifies data while passing through. | Compression (`zlib.createGzip()`)     |

| Method                    | Type     | Description                                                  |
| ------------------------- | -------- | ------------------------------------------------------------ |
| `.readable`               | Readable | Boolean: Indicates if the stream is open for reading.        |
| `.pipe(destination)`      | Readable | Pipes a readable stream into a writable stream.              |
| `.unpipe(destination)`    | Readable | Stops piping data to the destination stream.                 |
| `.write(chunk)`           | Writable | Writes data to a writable stream.                            |
| `.end([chunk])`           | Writable | Ends the writable stream (optionally writing a final chunk). |
| `.setEncoding(encoding)`  | Readable | Sets encoding for reading text data (`utf8`, `ascii`, etc.). |
| `.on("data", callback)`   | Readable | Listens for data events and receives chunks.                 |
| `.on("end", callback)`    | Readable | Fires when the stream finishes reading.                      |
| `.on("error", callback)`  | All      | Handles stream errors.                                       |
| `.on("finish", callback)` | Writable | Fires when a writable stream finishes writing.               |

```js
const fs = require("fs");

const readStream = fs.createReadStream("largeFile.txt", "utf8");

readStream.on("data", (chunk) => {
  console.log("Received chunk:", chunk);
});

readStream.on("end", () => {
  console.log("File reading completed.");
});

readStream.on("error", (err) => {
  console.error("Error reading file:", err);
});
```

## Zlib

The `zlib` module provides **compression and decompression** using algorithms like Gzip, Deflate, and Brotli.

| Method                                    | Type   | Description                                                     |
| ----------------------------------------- | ------ | --------------------------------------------------------------- |
| `zlib.createGzip()`                       | Stream | Creates a Gzip compression stream.                              |
| `zlib.createGunzip()`                     | Stream | Creates a stream to decompress Gzip data.                       |
| `zlib.createDeflate()`                    | Stream | Creates a Deflate compression stream.                           |
| `zlib.createInflate()`                    | Stream | Creates a stream to decompress Deflate data.                    |
| `zlib.createBrotliCompress()`             | Stream | Creates a Brotli compression stream (more efficient than Gzip). |
| `zlib.createBrotliDecompress()`           | Stream | Decompresses Brotli-compressed data.                            |
| `zlib.gzip(buffer, callback)`             | Async  | Compresses a buffer using Gzip.                                 |
| `zlib.gunzip(buffer, callback)`           | Async  | Decompresses a Gzip-compressed buffer.                          |
| `zlib.deflate(buffer, callback)`          | Async  | Compresses a buffer using Deflate.                              |
| `zlib.inflate(buffer, callback)`          | Async  | Decompresses a Deflate-compressed buffer.                       |
| `zlib.brotliCompress(buffer, callback)`   | Async  | Compresses a buffer using Brotli.                               |
| `zlib.brotliDecompress(buffer, callback)` | Async  | Decompresses a Brotli-compressed buffer.                        |

```js
const fs = require("fs");
const zlib = require("zlib");

const gzip = zlib.createGzip();
const input = fs.createReadStream("input.txt");
const output = fs.createWriteStream("input.txt.gz");

input.pipe(gzip).pipe(output);
```

## DNS

The DNS API allows Node.js applications to resolve domain names, perform reverse lookups, and manage DNS records.

| Method                                 | Description                                                 |
| -------------------------------------- | ----------------------------------------------------------- |
| `dns.lookup(hostname, callback)`       | Resolves a domain to an IP address (like `ping`).           |
| `dns.resolve4(hostname, callback)`     | Gets all IPv4 addresses for a domain.                       |
| `dns.resolve6(hostname, callback)`     | Gets all IPv6 addresses for a domain.                       |
| `dns.reverse(ip, callback)`            | Finds the hostname(s) for a given IP address.               |
| `dns.resolveMx(hostname, callback)`    | Retrieves mail exchange (MX) records for a domain.          |
| `dns.resolveTxt(hostname, callback)`   | Gets TXT records (used for domain verification, SPF, etc.). |
| `dns.resolveCname(hostname, callback)` | Retrieves the CNAME record for a domain.                    |
| `dns.resolveSrv(hostname, callback)`   | Gets SRV records (used for services like SIP, XMPP).        |
| `dns.promises.resolve4(hostname)`      | **(Recommended)** Returns IPv4 addresses as a Promise.      |
| `dns.promises.resolve6(hostname)`      | **(Recommended)** Returns IPv6 addresses as a Promise.      |

```js
const dns = require("dns");

dns.resolve4("example.com", (err, addresses) => {
  if (err) throw err;
  console.log(`IP addresses: ${addresses.join(", ")}`);
});
```

## Process

The Process API provides access to the currently running process, enabling interaction with system resources, environment variables, and process control.

| Method                                      | Description                                                |
| ------------------------------------------- | ---------------------------------------------------------- |
| `process.env`                               | Accesses environment variables (e.g., `process.env.PATH`). |
| `process.argv`                              | Retrieves command-line arguments passed to the script.     |
| `process.exit(code)`                        | Exits the process with a given exit code (default is `0`). |
| `process.on("exit", callback)`              | Executes a callback when the process exits.                |
| `process.on("uncaughtException", callback)` | Handles errors that would otherwise crash the process.     |
| `process.cwd()`                             | Gets the current working directory of the process.         |
| `process.chdir(path)`                       | Changes the working directory of the process.              |
| `process.uptime()`                          | Returns the process uptime in seconds.                     |
| `process.memoryUsage()`                     | Provides memory usage statistics.                          |
| `process.hrtime()`                          | Measures high-resolution time intervals.                   |

## JSDoc

It can also create a documentation.

| Tag           | Description                                    | Example                               |
| ------------- | ---------------------------------------------- | ------------------------------------- |
| `@param`      | Describes a function parameter                 | `@param {string} name - User's name`  |
| `@returns`    | Specifies the return value of a function       | `@returns {boolean} True if valid`    |
| `@typedef`    | Defines a custom type alias                    | `@typedef {Object} User`              |
| `@property`   | Describes properties inside an object          | `@property {string} email`            |
| `@example`    | Provides an example usage of the function      | `@example console.log(add(2,3));`     |
| `@throws`     | Indicates an error that a function might throw | `@throws {Error} If invalid`          |
| `@async`      | Marks a function as asynchronous               | `@async`                              |
| `@deprecated` | Marks a function as outdated                   | `@deprecated Use newMethod()`         |
| `@see`        | References related documentation               | `@see https://developer.mozilla.org/` |
| `@private`    | Marks a method as private                      | `@private`                            |
| `@readonly`   | Marks a property as read-only                  | `@readonly`                           |
| `@interface`  | Defines an interface for objects               | `@interface UserProfile`              |

## CLI

| Flag/Env Variable              | Description                                        | Example Usage                                          |
| ------------------------------ | -------------------------------------------------- | ------------------------------------------------------ |
| `--experimental`               | Enables experimental features                      | `node --experimental-vm-modules app.js`                |
| `--require` / `-r`             | Preloads modules before execution                  | `node -r dotenv/config app.js`                         |
| `--inspect[=host:port]`        | Enables debugging with Chrome DevTools             | `node --inspect app.js`                                |
| `--inspect-brk`                | Pauses execution until a debugger is attached      | `node --inspect-brk app.js`                            |
| `--trace-warnings`             | Shows stack traces for process warnings            | `node --trace-warnings app.js`                         |
| `--max-old-space-size=<MB>`    | Sets the max memory usage for V8 heap              | `node --max-old-space-size=4096 app.js`                |
| `--no-deprecation`             | Suppresses deprecation warnings                    | `node --no-deprecation app.js`                         |
| `--trace-deprecation`          | Shows stack traces for deprecated functions        | `node --trace-deprecation app.js`                      |
| `NODE_ENV`                     | Defines the environment (development, production)  | `NODE_ENV=production node app.js`                      |
| `NODE_OPTIONS`                 | Passes runtime options to Node.js                  | `NODE_OPTIONS="--max-old-space-size=2048" node app.js` |
| `NODE_PATH`                    | Sets module resolution paths                       | `NODE_PATH=/usr/local/lib/node_modules node app.js`    |
| `NODE_NO_WARNINGS`             | Disables all warnings                              | `NODE_NO_WARNINGS=1 node app.js`                       |
| `NODE_EXTRA_CA_CERTS`          | Loads extra CA certificates                        | `NODE_EXTRA_CA_CERTS=/path/to/cert.pem node app.js`    |
| `--watch`                      | Automatically restarts the process on file changes | `node --watch app.js`                                  |
| `--title=<title>`              | Sets a custom process title                        | `node --title="MyApp" app.js`                          |
| `--eval "code"` / `-e "code"`  | Executes JavaScript from the command line          | `node -e "console.log(2 + 2)"`                         |
| `--print "code"` / `-p "code"` | Evaluates code and prints the result               | `node -p "Math.random()"`                              |
| `--check` / `-c`               | Syntax-checks the script without execution         | `node --check app.js`                                  |

## NPM

| Command                    | Description                                    | Example Usage             |
| -------------------------- | ---------------------------------------------- | ------------------------- |
| `npm init`                 | Initializes a new package.json                 | `npm init -y`             |
| `npm install <package>`    | Installs a package locally                     | `npm install express`     |
| `npm install -g <package>` | Installs a package globally                    | `npm install -g nodemon`  |
| `npm uninstall <package>`  | Removes a package                              | `npm uninstall express`   |
| `npm update <package>`     | Updates a package to the latest version        | `npm update lodash`       |
| `npm list`                 | Lists installed dependencies                   | `npm list`                |
| `npm list -g`              | Lists globally installed packages              | `npm list -g`             |
| `npm run <script>`         | Runs a custom script defined in package.json   | `npm run start`           |
| `npm publish`              | Publishes a package to the npm registry        | `npm publish`             |
| `npm login`                | Logs into the npm registry                     | `npm login`               |
| `npm cache clean --force`  | Clears npm cache                               | `npm cache clean --force` |
| `npm audit`                | Checks for security vulnerabilities            | `npm audit`               |
| `npm audit fix`            | Fixes security issues automatically            | `npm audit fix`           |
| `npm ci`                   | Installs dependencies from `package-lock.json` | `npm ci`                  |

## package.json

| Option                   | Type      | Description                                                                    |
| ------------------------ | --------- | ------------------------------------------------------------------------------ |
| `"name"`                 | `string`  | The name of the package (must be lowercase and URL-friendly).                  |
| `"version"`              | `string`  | The version of the package (follows Semantic Versioning: `major.minor.patch`). |
| `"description"`          | `string`  | A short description of the package.                                            |
| `"main"`                 | `string`  | The entry point file of the package (e.g., `"index.js"`).                      |
| `"type"`                 | `string`  | Defines the module system: `"commonjs"` (default) or `"module"` (for ESM).     |
| `"scripts"`              | `object`  | Defines shortcut commands that can be run via `npm run <script-name>`.         |
| `"dependencies"`         | `object`  | Lists production dependencies and their versions.                              |
| `"devDependencies"`      | `object`  | Lists development dependencies.                                                |
| `"peerDependencies"`     | `object`  | Defines dependencies that should be installed by the consumer of the package.  |
| `"optionalDependencies"` | `object`  | Defines dependencies that can be skipped if installation fails.                |
| `"engines"`              | `object`  | Specifies required Node.js and npm versions.                                   |
| `"private"`              | `boolean` | If `true`, prevents the package from being published to npm.                   |
| `"license"`              | `string`  | Defines the license type (e.g., `"MIT"`, `"ISC"`).                             |
| `"repository"`           | `object`  | Contains metadata about the repository (e.g., GitHub URL).                     |
| `"bugs"`                 | `object`  | URL for reporting package issues (e.g., GitHub Issues link).                   |
| `"homepage"`             | `string`  | URL of the package homepage.                                                   |
| `"bin"`                  | `object`  | Defines executable scripts that can be run globally.                           |
| `"files"`                | `array`   | Lists files to include when publishing the package.                            |
| `"config"`               | `object`  | Stores custom configuration values accessible in scripts.                      |
| `"workspaces"`           | `array`   | Defines monorepo workspace directories.                                        |
| `"exports"`              | `object`  | Defines entry points for different module formats.                             |
| `"types"`                | `string`  | Specifies the TypeScript declaration file (`.d.ts`).                           |
| `"author"`               | `string`  | Author's name and contact details.                                             |

## npm scripts

| Feature                         | Description                                                                         |
| ------------------------------- | ----------------------------------------------------------------------------------- |
| Custom Scripts                  | Define commands in `package.json` to run tasks.                                     |
| Built-in Lifecycle Scripts      | Automatically triggered scripts like `preinstall`, `postinstall`, `pretest`, etc.   |
| Running Scripts Sequentially    | Use `&&` to chain commands.                                                         |
| Running Scripts in Parallel     | Use `&` (Unix/macOS) or `start` (Windows).                                          |
| Environment Variables           | Define runtime variables within scripts (`set` for Windows, inline for Unix/macOS). |
| `npm-run-all` Utility           | Runs multiple scripts concurrently.                                                 |
| Pre and Post Hooks              | Prefix scripts with `pre` and `post` for automated execution.                       |
| Using Local `node_modules/.bin` | Automatically resolves executables without full paths.                              |
| Passing Arguments               | Use `--` to pass extra arguments to scripts.                                        |
| Common Shortcuts                | `npm start`, `npm test`, `npm restart` simplify script execution.                   |
| Executing External Scripts      | Run external Node.js files via npm scripts.                                         |

## profiling

Start the app with `--prof` then make some load tests with e.g. ApacheBench.

## JWT

| Section                            | Description                                                                                                                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **What is JWT**                    | JWT (JSON Web Token) is an open standard (RFC 7519) used to securely transmit information as a JSON object.                                                                                                   |
| **Structure**                      | A JWT consists of three parts: `HEADER.PAYLOAD.SIGNATURE`.                                                                                                                                                    |
| **Header**                         | Metadata about the token (e.g., signing algorithm and token type).                                                                                                                                            |
| **Payload**                        | Contains claims (data) like `iss`, `sub`, `exp`, and custom claims.                                                                                                                                           |
| **Signature**                      | Encodes the header and the payload with base64 then using my secret it calls the defined signing algorithm. If the header or payload is changed, we can notice it by comparing them to the signature.         |
| **Common Claims**                  | `iss` (issuer), `sub` (subject), `aud` (audience), `exp` (expiration), `nbf` (not before), `iat` (issued at), `jti` (JWT ID).                                                                                 |
| **Signing Algorithms**             | - `HS256` – HMAC with SHA-256 (Symmetric) <br> - `RS256` – RSA with SHA-256 (Asymmetric) <br> - `ES256` – ECDSA with SHA-256 (Asymmetric)                                                                     |
| **How JWT Works**                  | 1. User logs in → Server creates a JWT and sends it to the client. <br> 2. Client stores JWT and includes it in requests (`Authorization` header). <br> 3. Server verifies the JWT and processes the request. |
| **Example (Using `jsonwebtoken`)** | `const token = jwt.sign({ userId: 123 }, secret, { expiresIn: '1h' });`                                                                                                                                       |
| **Advantages**                     | ✅ Stateless <br> ✅ Compact <br> ✅ Portable                                                                                                                                                                 |
| **Disadvantages**                  | ❌ Can't be invalidated easily <br> ❌ Large payloads increase token size <br> ❌ Security risks if mishandled                                                                                                |
| **Security Best Practices**        | - Use HTTPS <br> - Short expiration times <br> - Store securely (HttpOnly, Secure cookies) <br> - Validate tokens properly                                                                                    |
| **Refresh token**                  | There can be a refresh token too. If the access token lasts only for like 15 mins, then we can send our refresh token to the server to get a new access token.                                                |

## HTTPS

[Source](https://www.youtube.com/watch?v=j9QmMEWmcfo)
![Event loop](/assets/https.png)

1. TCP handshake

- client connects to the web server

2. Certificate check

- client asks what protocol, alorithm, etc is supported
- server answers
- client checks the certificate of the server using authorities
- server send its public key

3. Key exchange

- client creates a session key and encrypts it with the server's public key
- client send the encrypted key to the server
- server decrypts the key with its private key

4. Data transmission

- now bot of the can use the session key to encryt data

TLS handshake: when they exchange the session key used to encrypt the data.

## Passport.js

| Section                 | Description                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **What is Passport.js** | Passport.js is a flexible authentication middleware for Node.js that supports various authentication methods, including Local, OAuth, JWT, and OpenID.                                                                                                                                                                                                            |
| **Core Concepts**       | - **Strategy** – Method for authenticating users (e.g., Local, OAuth). <br> - **Session** – Stores user data in a session or returns a token. <br> - **Authenticate** – Function used to authenticate requests. <br> - **Serialization** – Converts user data to session format. <br> - **Deserialization** – Converts session data back to a user object.        |
| **How It Works**        | 1. User sends login request. <br> 2. Passport processes request using a strategy. <br> 3. If successful, Passport serializes user and starts a session. <br> 4. Subsequent requests deserialize user and attach it to `req.user`.                                                                                                                                 |
| **Example Code**        | Install: `npm install passport passport-local` <br> Local strategy example: <br>`js<br>passport.use(new LocalStrategy((username, password, done) => {<br> if (username === 'admin' && password === 'secret') {<br> return done(null, { id: 1, name: 'Admin' });<br> } else {<br> return done(null, false, { message: 'Invalid credentials' });<br> }<br>}));<br>` |
| **Common Strategies**   | - **Local** – Authenticate using username and password (`passport-local`) <br> - **OAuth** – Authenticate using Google, Facebook, Twitter, etc. <br> - **JWT** – Token-based authentication (`passport-jwt`) <br> - **OpenID** – OpenID Connect-based authentication (`passport-openidconnect`) <br> - **Custom** – Create your own strategy                      |

```js
// 1. setting up local strategy
const passport = require("passport");
const LocalStrategy = require("passport-local").Strategy;

passport.use(
  new LocalStrategy((username, password, done) => {
    if (username === "admin" && password === "secret") {
      return done(null, { id: 1, name: "Admin" });
    } else {
      return done(null, false, { message: "Invalid credentials" });
    }
  })
);

passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser((id, done) => {
  // Look up user in DB here
  done(null, { id: 1, name: "Admin" });
});

// 2. login
const express = require("express");
const app = express();

app.post(
  "/login",
  passport.authenticate("local", {
    successRedirect: "/dashboard",
    failureRedirect: "/login",
    failureFlash: true,
  })
);

// 3. protected route
const ensureAuthenticated = (req, res, next) => {
  if (req.isAuthenticated()) {
    return next();
  }
  res.redirect("/login");
};

app.get("/dashboard", ensureAuthenticated, (req, res) => {
  res.send(`Welcome, ${req.user.name}`);
});
```

## Helmet

| Feature                       | Description                                                                                                                                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **What is Helmet.js**         | Helmet.js is a middleware for Express.js that helps secure apps by setting HTTP headers. It mitigates common web vulnerabilities like XSS, clickjacking, and sniffing.                                |
| **hidePoweredBy**             | Removes the `X-Powered-By` header to prevent attackers from knowing the app framework.                                                                                                                |
| **contentSecurityPolicy**     | Sets `Content-Security-Policy` header to restrict the sources of content that can be loaded (like scripts, styles, images).                                                                           |
| **xssFilter**                 | Sets `X-XSS-Protection` header to enable browser XSS protection.                                                                                                                                      |
| **frameguard**                | Sets `X-Frame-Options` header to prevent clickjacking by not allowing the site to be embedded in iframes.                                                                                             |
| **noSniff**                   | Sets `X-Content-Type-Options` header to prevent the browser from trying to guess MIME types.                                                                                                          |
| **ieNoOpen**                  | Sets `X-Download-Options` header to prevent downloads from executing in IE.                                                                                                                           |
| **hsts**                      | Sets `Strict-Transport-Security` header to enforce HTTPS connections.                                                                                                                                 |
| **dnsPrefetchControl**        | Controls browser DNS prefetching using `X-DNS-Prefetch-Control` header.                                                                                                                               |
| **referrerPolicy**            | Sets `Referrer-Policy` header to control the referrer information sent with requests.                                                                                                                 |
| **expectCt**                  | Sets `Expect-CT` header to handle Certificate Transparency requirements.                                                                                                                              |
| **crossOriginEmbedderPolicy** | Sets `Cross-Origin-Embedder-Policy` to restrict how a document can be embedded.                                                                                                                       |
| **crossOriginOpenerPolicy**   | Sets `Cross-Origin-Opener-Policy` to protect against cross-origin attacks.                                                                                                                            |
| **crossOriginResourcePolicy** | Sets `Cross-Origin-Resource-Policy` to control who can load resources from your domain.                                                                                                               |
| **Example Code**              | `js<br>const helmet = require('helmet');<br>const express = require('express');<br>const app = express();<br>app.use(helmet());<br>app.listen(3000);<br>`                                             |
| **Best Practices**            | ✅ Enable only the headers that are needed for the app. <br> ✅ Adjust `Content-Security-Policy` to allow necessary sources. <br> ✅ Ensure HSTS is set up correctly to avoid mixed content warnings. |
| **Advantages**                | ✅ Easy to set up <br> ✅ Covers a wide range of security issues <br> ✅ Highly configurable                                                                                                          |
| **Disadvantages**             | ❌ Can conflict with some frontend frameworks (due to CSP) <br> ❌ Needs proper configuration for complex apps                                                                                        |

### Headers

| Header                                  | Description                                                                                       | Example Value                                                     | Purpose                                                                   |
| --------------------------------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **X-Powered-By**                        | Identifies the framework used (e.g., Express). Removing it hides this information from attackers. | `Express`                                                         | Hides framework details to prevent targeted attacks.                      |
| **Content-Security-Policy (CSP)**       | Controls which resources (scripts, styles, images) can be loaded.                                 | `default-src 'self'; script-src 'self' https://apis.example.com`  | Prevents XSS and data injection attacks.                                  |
| **X-XSS-Protection**                    | Enables browser-based XSS filtering.                                                              | `1; mode=block`                                                   | Stops browsers from loading pages when they detect reflected XSS attacks. |
| **X-Frame-Options**                     | Prevents the site from being embedded in an iframe.                                               | `DENY` or `SAMEORIGIN`                                            | Protects against clickjacking.                                            |
| **X-Content-Type-Options**              | Forces the browser to follow the MIME type declared by the server.                                | `nosniff`                                                         | Prevents MIME type sniffing attacks.                                      |
| **X-Download-Options**                  | Instructs Internet Explorer not to execute downloads in its context.                              | `noopen`                                                          | Reduces risk of executing malicious files.                                |
| **Strict-Transport-Security (HSTS)**    | Forces the browser to use HTTPS connections only.                                                 | `max-age=63072000; includeSubDomains`                             | Ensures secure communication over HTTPS.                                  |
| **X-DNS-Prefetch-Control**              | Controls whether the browser should perform DNS prefetching.                                      | `off` or `on`                                                     | Reduces DNS-based attacks and improves privacy.                           |
| **Referrer-Policy**                     | Controls how much referrer information is included with requests.                                 | `no-referrer`, `same-origin`, `strict-origin-when-cross-origin`   | Protects user privacy and prevents information leaks.                     |
| **Expect-CT**                           | Controls how Certificate Transparency violations are handled.                                     | `max-age=86400, enforce, report-uri="https://example.com/report"` | Ensures valid certificates and prevents misissued certificates.           |
| **Cross-Origin-Embedder-Policy (COEP)** | Controls how a document can embed resources from other origins.                                   | `require-corp`                                                    | Protects against side-channel attacks like Spectre.                       |
| **Cross-Origin-Opener-Policy (COOP)**   | Controls how a document interacts with others in different origins.                               | `same-origin`                                                     | Isolates the browsing context to prevent data leaks.                      |
| **Cross-Origin-Resource-Policy (CORP)** | Controls who can load resources from the domain.                                                  | `same-origin`, `cross-origin`, `same-site`                        | Prevents unauthorized loading of resources.                               |

## Security best practices

| Best Practice                                        | Description                                                                                   | Example                                                                                                       | Purpose                                        |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **Use `helmet` for securing HTTP headers**           | Helmet helps protect the app from common vulnerabilities by setting appropriate HTTP headers. | `app.use(helmet())`                                                                                           | Prevents XSS, clickjacking, and other attacks. |
| **Avoid using `eval()`**                             | `eval()` allows execution of arbitrary code, which can lead to RCE (Remote Code Execution).   | `const result = eval(userInput)` ❌                                                                           | Prevents code injection attacks.               |
| **Use `crypto` for hashing and encryption**          | Use the Node.js `crypto` module for secure encryption and hashing.                            | `crypto.createHash('sha256').update(data).digest('hex')`                                                      | Ensures secure handling of sensitive data.     |
| **Avoid serving sensitive data in error messages**   | Error messages can reveal sensitive information to attackers.                                 | `app.use((err, req, res, next) => { res.status(500).send('Internal Server Error'); })`                        | Prevents information leakage.                  |
| **Sanitize and validate user input**                 | Use libraries like `validator` to validate and sanitize input.                                | `validator.isEmail(req.body.email)`                                                                           | Prevents XSS and injection attacks.            |
| **Use HTTPS**                                        | Always serve the app over HTTPS to encrypt data in transit.                                   | `https.createServer({ key, cert }, app).listen(443)`                                                          | Prevents man-in-the-middle attacks.            |
| **Limit payload size**                               | Restrict the size of incoming requests to prevent denial of service (DoS).                    | `app.use(express.json({ limit: '1mb' }))`                                                                     | Prevents DoS through large payloads.           |
| **Avoid `process.env` leaks**                        | Never expose `process.env` values to the client-side.                                         | `const secret = process.env.SECRET`                                                                           | Protects sensitive data like API keys.         |
| **Use `Content-Security-Policy` (CSP)**              | Restrict sources of scripts and styles to prevent XSS.                                        | `helmet.contentSecurityPolicy({ directives: { defaultSrc: ["'self'"] } })`                                    | Mitigates XSS attacks.                         |
| **Avoid hardcoded secrets**                          | Store sensitive data in environment variables or a secure vault.                              | `.env` file with `API_KEY=secret_key`                                                                         | Protects sensitive configuration data.         |
| **Enable logging and monitoring**                    | Use logging libraries like `winston` or `bunyan` to track activity and failures.              | `const logger = require('winston')`                                                                           | Helps identify suspicious activity.            |
| **Implement rate limiting**                          | Limit the number of requests per IP to prevent brute-force attacks.                           | `const rateLimit = require('express-rate-limit'); app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }))` | Prevents DoS and brute-force attacks.          |
| **Avoid `child_process` with untrusted input**       | Never pass user input directly into `child_process` methods.                                  | `const exec = require('child_process').exec; exec(userInput)` ❌                                              | Prevents command injection.                    |
| **Use the latest LTS version of Node.js**            | Keep Node.js and dependencies updated to avoid vulnerabilities.                               | `nvm install --lts`                                                                                           | Fixes security bugs and vulnerabilities.       |
| **Set proper permissions for files and directories** | Ensure that files and folders have minimal necessary permissions.                             | `chmod 600 .env`                                                                                              | Prevents unauthorized access.                  |
| **Avoid wildcard imports (`*`)**                     | Import only what's needed to minimize the attack surface.                                     | `import { specificFunction } from 'module'`                                                                   | Reduces unintended exposure.                   |

## Microservice

| Feature                           | Description                                                                   | Example                                                                              | Pros                                                          | Cons                                                               |
| --------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Independent Deployment**        | Each microservice can be deployed independently without affecting others.     | Updating the user service without touching the payment service.                      | Faster updates and easier rollbacks.                          | Increased complexity in managing multiple services.                |
| **Decentralized Data Management** | Each microservice can manage its own database or storage.                     | A product service using MongoDB and an order service using PostgreSQL.               | Improved scalability and flexibility.                         | Data consistency challenges and duplication.                       |
| **Technology Agnostic**           | Microservices can use different programming languages and frameworks.         | A service written in Node.js communicating with a service written in Python.         | Freedom to choose the best tool for the job.                  | More complex infrastructure and monitoring.                        |
| **Fault Isolation**               | A failure in one microservice doesn't bring down the whole system.            | A cart service fails, but checkout still works.                                      | Increased resilience and uptime.                              | Requires careful handling of inter-service communication failures. |
| **Scalability**                   | Individual microservices can scale independently based on demand.             | Scaling the product catalog service separately from the user authentication service. | Efficient use of resources and better performance under load. | Load balancing and network complexity.                             |
| **Granular Services**             | Services are designed to be small and focused on a single function.           | A separate login service, a user profile service, and a session management service.  | Clean separation of concerns and improved modularity.         | Too many small services can increase maintenance effort.           |
| **API-First Approach**            | Microservices expose APIs for communication using HTTP, gRPC, or messaging.   | A REST API for fetching product data.                                                | Clear interface for integration and testing.                  | API versioning and backward compatibility challenges.              |
| **Asynchronous Communication**    | Microservices can communicate through message queues (e.g., Kafka, RabbitMQ). | A payment service publishes a message when payment is complete.                      | Improved performance and decoupling.                          | Requires additional infrastructure and monitoring.                 |
| **Service Discovery**             | Services register themselves and discover each other dynamically.             | Using tools like Consul or Eureka.                                                   | Reduces configuration effort and allows dynamic scaling.      | Adds complexity and potential network latency.                     |
| **Load Balancing**                | Requests are distributed evenly across service instances.                     | A load balancer routes requests to the least busy instance.                          | Better resource utilization and higher availability.          | Can introduce additional latency and configuration complexity.     |

## OAuth

![Event loop](/assets/oauth.png)
[Source](https://www.youtube.com/watch?v=ZV5yTm4pT8g)

- we have photos in the SnapStore
- we want PrintMagic to print our photos but we don't want to upload our images manually nor we want to add our username and password to PrintMagic
- PrintMagic has already registered itself to SnapStore - it is like that PrintMagic is a "user" of Snapstore: PrintMagic has a **client id** and a **client secret** from the SnapStore that identifies PrintMagic
- when we want to print with PrintMagic, we are redirected to the login screen of SnapStore where we can see what stuff PrintMagic wants access to (our photos in this case) - **client id** and **scope** (list of the stuff PrintMagic wants) are sent
- if we approve the request, PrintMagic gets an **authorization code** from SnapStore
- SnapStore now tries to get an **access token** from SnapStore by sending the **client id**, **client secret** and **authorization code**

<hr>

- As a first step you need to register your app with the OAuth provider. You need to define a redirect_url which is a URL the browser will be redirected to with the authorization code in the URL. During the registration process you get a client_id and a client_secret - these are like the username + password of your app to the OAuth provider.

- The user clicks on your login button.

- The user is redirected to the 3rd party OAuth provider login page. You have to send your client_id in order to open that page.

- After the successful login the browser is redirected to the URL you specified at the registration. The authorization code is received in the URL. This is important: the user initiates the login process form the browser so when the OAuth provider makes a redirection it redirects your browser!

- The authorization code is in the browser URL so it is not really safe it can be stolen. This is why the temporary authorization code is received at this step and not the access token.

- So now your browser makes a request to your backend code with the authorization code .

- Your backend can now ask for an access token from the OAuth provider using the authorization code + the client_id + client_secret.

- Your client_secret lives only on your server so even if the attacker stole the authorization code the attacker is would not be able to create an access token because the attacker would need to have access to your client_secret too!

## NPM CLI

| Command                            | Description                                                                   | Example                                      |
| ---------------------------------- | ----------------------------------------------------------------------------- | -------------------------------------------- |
| `npm init`                         | Initializes a new `package.json` file in the current directory.               | `npm init`                                   |
| `npm install <package>`            | Installs a package and adds it to `dependencies` in `package.json`.           | `npm install express`                        |
| `npm install <package> --save-dev` | Installs a package and adds it to `devDependencies` in `package.json`.        | `npm install jest --save-dev`                |
| `npm install`                      | Installs all dependencies listed in `package.json`.                           | `npm install`                                |
| `npm update`                       | Updates all packages to the latest version compatible with `package.json`.    | `npm update`                                 |
| `npm uninstall <package>`          | Removes a package and its reference from `dependencies` or `devDependencies`. | `npm uninstall express`                      |
| `npm list`                         | Lists installed packages and their dependencies.                              | `npm list`                                   |
| `npm run <script>`                 | Runs a script defined in `package.json`.                                      | `npm run build`                              |
| `npm test`                         | Runs the test script defined in `package.json`.                               | `npm test`                                   |
| `npm start`                        | Runs the start script defined in `package.json`.                              | `npm start`                                  |
| `npm audit`                        | Scans dependencies for known security vulnerabilities.                        | `npm audit`                                  |
| `npm audit fix`                    | Attempts to automatically fix security vulnerabilities.                       | `npm audit fix`                              |
| `npm cache clean --force`          | Clears the npm cache.                                                         | `npm cache clean --force`                    |
| `npm login`                        | Logs into npm to publish or access private packages.                          | `npm login`                                  |
| `npm publish`                      | Publishes a package to the npm registry.                                      | `npm publish`                                |
| `npm whoami`                       | Displays the currently logged-in user.                                        | `npm whoami`                                 |
| `npm config set <key> <value>`     | Sets a config value.                                                          | `npm config set init-author-name "John Doe"` |
| `npm config get <key>`             | Displays a config value.                                                      | `npm config get init-author-name`            |
| `npm link`                         | Creates a symlink to a local package for development purposes.                | `npm link`                                   |
| `npm unlink`                       | Removes the symlink created by `npm link`.                                    | `npm unlink`                                 |

## V8 optimization

| Technique                          | Description                                                                                                                 | Benefit                                            |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| **Inline Caching**                 | V8 tracks the type of objects being accessed and optimizes repeated calls by caching the lookup results.                    | Speeds up property access and method calls.        |
| **Hidden Classes**                 | V8 creates hidden classes internally to represent object shapes, which allows fast property access.                         | Reduces dynamic property lookup time.              |
| **Just-In-Time (JIT) Compilation** | V8 compiles JavaScript code into machine code at runtime rather than interpreting it.                                       | Improves execution speed.                          |
| **Optimizing Hot Code**            | V8 monitors frequently executed code and re-optimizes it using type information and hidden classes.                         | Enhances performance for common operations.        |
| **Deoptimization**                 | If assumptions made by the optimizer are invalidated (e.g., type change), V8 falls back to the interpreter.                 | Ensures correctness when dynamic types change.     |
| **Garbage Collection**             | V8 uses a generational garbage collector (new and old spaces) to efficiently manage memory.                                 | Reduces memory usage and prevents memory leaks.    |
| **Lazy Compilation**               | Functions are compiled only when they are first called, rather than when the script loads.                                  | Reduces startup time and memory consumption.       |
| **Escape Analysis**                | V8 determines whether an object is accessible outside a function; if not, it allocates it on the stack instead of the heap. | Reduces garbage collection overhead.               |
| **Function Inlining**              | Frequently used small functions are directly inserted into the calling function’s code.                                     | Reduces function call overhead.                    |
| **Optimizing Arrays**              | V8 treats small arrays of the same type as "packed" arrays, making them faster to access.                                   | Improves array performance.                        |
| **Code Caching**                   | Compiled machine code is cached for faster subsequent execution.                                                            | Reduces loading time for frequently executed code. |
| **Map Transitions**                | When object shapes change, V8 creates a new hidden class and caches the transition.                                         | Reduces property lookup time for evolving objects. |
