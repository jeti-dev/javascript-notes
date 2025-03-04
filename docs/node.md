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
| **Top-Level `await`**     | ❌ Not supported                                 | ✅ Supported                                                       |
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

## Async timing - more to come

Comparison of Asynchronous Execution Methods in Node.js

| **Method**           | **Execution Timing**                                       | **Order in Event Loop**                                            | **Use Case**                                                                 |
| -------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| `process.nextTick()` | Before I/O, timers, and `setImmediate()`                   | **Highest Priority** (executes immediately after synchronous code) | Critical tasks that must execute before moving to the next event loop cycle. |
| `queueMicrotask()`   | Before next event loop phase, after resolved Promises      | **After `process.nextTick()` but before I/O**                      | Short async tasks that shouldn't block the event loop.                       |
| `setImmediate()`     | After I/O callbacks, before next event loop cycle          | **Runs after I/O callbacks**                                       | Tasks that should run after I/O but before the next loop iteration.          |
| `setTimeout(fn, 0)`  | After at least the specified delay (0 ms means next cycle) | **Runs after timers phase**                                        | Delayed execution, runs in the **timers phase** after I/O.                   |

```js
console.log("Start");

setTimeout(() => console.log("setTimeout"), 0);
setImmediate(() => console.log("setImmediate"));

process.nextTick(() => console.log("nextTick"));
queueMicrotask(() => console.log("queueMicrotask"));

console.log("End");

// Start
// End
// nextTick
// queueMicrotask
// setImmediate
// setTimeout
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

The `string_decoder` module is used to decode buffers into strings **without losing multi-byte characters**.  
It is ideal for **streamed or chunked data processing** where characters may be split across chunks.

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
However `woorker_threads` should be used when process isolation is not needed. `worker_threads` allows running multiple application threads within a single Node.js instance.
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
| `Atomics`                            | Ensures safe concurrent memory operations                | Synchronizing access to shared memory                      |

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
