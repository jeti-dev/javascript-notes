---
title: Web
layout: default
---

# Web

## Performance

- FCP - first contenful paint: the first stuff the browser draws

- LCP - largest contentful paint: after my images and videos loaded
- - should be under 2,5s, over 4s is not good for SEO
- - compress images, use only a few fonts
- - use CDN
- - server side rendering is faster than doing some stuff first and only after getting the images
- - user rel="preload" to preload assets
- - fetchpriority="high/low"

- FID - first input delay: how much time it takes for the event to run when the user first interacts with the app
- - should be under 100ms, over 300ms is very bad
- - js: reduce execute time - lazy loading, web workers

- CLS - cummulative layout shift: elements on the page shouldn't jump around unexpectedly
- - use width and height for images or srcset (different sized images based on condition)

### Tips

| Technique                                | Description                                                                                | Benefit                                                             |
| ---------------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| **Minify CSS, JavaScript, and HTML**     | Remove unnecessary whitespace, comments, and characters from files.                        | Reduces file size, improving load times.                            |
| **Use Gzip or Brotli Compression**       | Compress assets (HTML, CSS, JS) before sending them to the browser.                        | Reduces the amount of data transferred over the network.            |
| **Optimize Images**                      | Use modern formats (e.g., WebP), resize images, and compress them.                         | Reduces image size without losing quality, improving load times.    |
| **Lazy Loading**                         | Load images and other resources only when they’re needed (as they come into the viewport). | Reduces initial load time and improves perceived performance.       |
| **Use a Content Delivery Network (CDN)** | Distribute static assets across multiple geographic locations.                             | Reduces latency and improves load times for global users.           |
| **Reduce HTTP Requests**                 | Combine CSS and JS files, use CSS sprites, and inline small assets.                        | Reduces the number of network requests, speeding up page rendering. |
| **Enable HTTP/2 or HTTP/3**              | Use multiplexing to send multiple requests over a single connection.                       | Reduces connection overhead and improves resource loading.          |
| **Use Efficient Caching**                | Set appropriate `Cache-Control` and `ETag` headers.                                        | Reduces the need to re-download unchanged assets.                   |
| **Preload and Prefetch**                 | Use `<link rel="preload">` and `<link rel="prefetch">` to load critical resources earlier. | Reduces waiting time for key resources.                             |
| **Reduce Render-Blocking Resources**     | Minimize or defer CSS and JS that block page rendering.                                    | Improves First Contentful Paint (FCP).                              |
| **Use Code Splitting**                   | Split JavaScript into smaller chunks and load them on demand.                              | Reduces initial bundle size and speeds up the main thread.          |
| **Eliminate Unused CSS and JavaScript**  | Remove unused styles and scripts from the project.                                         | Reduces file size and improves parsing time.                        |
| **Use Responsive Images**                | Serve different images based on screen size and resolution.                                | Improves performance on mobile and high-density screens.            |
| **Defer Third-Party Scripts**            | Load third-party scripts (like ads and analytics) asynchronously.                          | Prevents them from blocking the main thread.                        |
| **Reduce DOM Size**                      | Minimize the number of DOM elements and avoid deep nesting.                                | Improves browser rendering efficiency.                              |
| **Reduce Time to First Byte (TTFB)**     | Optimize server response time and database queries.                                        | Improves perceived performance and load speed.                      |
| **Use Service Workers**                  | Cache assets and handle network requests in the background.                                | Enables offline support and faster repeat visits.                   |
| **Reduce Redirects**                     | Minimize the number of redirects (e.g., `www → non-www`).                                  | Reduces round trips and improves TTFB.                              |
| **Use Web Workers**                      | Offload heavy computations to background threads.                                          | Prevents blocking the main thread, improving responsiveness.        |

### Repaint, reflow, critical rendering path

| Concept                           | Description                                                                                           | Causes                                                                                                 | Performance Impact                             | Example                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Critical Rendering Path (CRP)** | The sequence of steps the browser takes to convert HTML, CSS, and JS into pixels on the screen.       | Parsing HTML, CSS, and JS; Building DOM and CSSOM; Creating Render Tree; Layout; Painting; Compositing | High                                           | - Parsing HTML and CSS<br>- Building render tree<br>- Painting elements                     |
| **Reflow (Layout)**               | Recalculates the size and position of elements in the document due to changes that affect the layout. | Changes to `width`, `height`, `margin`, `padding`, `position`, `font-size`, etc.                       | High (causes full re-render of affected areas) | `js<br>element.style.width = "300px";<br>element.style.margin = "20px";<br>`                |
| **Repaint**                       | Redrawing elements without changing layout (e.g., visual properties like color).                      | Changes to `background-color`, `visibility`, `box-shadow`, etc.                                        | Medium (less expensive than reflow)            | `js<br>element.style.backgroundColor = "red";<br>`                                          |
| **Compositing**                   | Combining separate layers (after painting) into the final visual frame.                               | `transform`, `opacity`, `z-index`                                                                      | Low (GPU-optimized)                            | `js<br>element.style.transform = "translateX(50px)";<br>element.style.opacity = "0.5";<br>` |

#### ✅ **Performance Tips**

- Minimize reflows by batching DOM changes.
- Prefer `transform` and `opacity` over direct layout changes.
- Use `requestAnimationFrame()` for animations.
- Use `classList.add()` instead of modifying CSS properties one by one.
- Avoid setting `innerHTML` repeatedly.

### Tips

| Area         | Technique                                      | Description                                                            | Impact | Example                                                      |
| ------------ | ---------------------------------------------- | ---------------------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| **Frontend** | **Minify CSS, JS, and HTML**                   | Reduces file size by removing unnecessary characters and whitespace.   | High   | Use tools like `Terser` for JS and `cssnano` for CSS.        |
| **Frontend** | **Lazy Loading**                               | Defers loading of non-essential resources until they are needed.       | High   | `html<br><img loading="lazy" src="image.jpg" />`             |
| **Frontend** | **Code Splitting**                             | Breaks large JS files into smaller chunks to load only what's needed.  | High   | Use Webpack’s `import()` for dynamic imports.                |
| **Frontend** | **Use Content Delivery Network (CDN)**         | Serve static assets from servers close to the user.                    | High   | Use `Cloudflare` or `AWS CloudFront`.                        |
| **Frontend** | **Reduce HTTP Requests**                       | Combine files and use image sprites to reduce server requests.         | Medium | Combine CSS files and use `data URIs` for small images.      |
| **Frontend** | **Use Gzip/Brotli Compression**                | Compress resources before sending them to the client.                  | High   | Configure Nginx or Apache to enable compression.             |
| **Frontend** | **Optimize Critical Rendering Path (CRP)**     | Optimize the loading order of CSS, JS, and HTML.                       | High   | Load CSS before JS and minimize render-blocking resources.   |
| **Frontend** | **Use HTTP/2 or HTTP/3**                       | Enables multiplexing and faster parallel requests.                     | High   | Configure server to support HTTP/2 or HTTP/3.                |
| **Frontend** | **Reduce Render-Blocking Resources**           | Load JS and CSS asynchronously where possible.                         | High   | `html<br><link rel="preload" href="style.css" as="style" />` |
| **Frontend** | **Image Optimization**                         | Compress and resize images for faster loading.                         | High   | Use `WebP` or `AVIF` format instead of JPEG or PNG.          |
| **Frontend** | **Use `requestAnimationFrame()`**              | Synchronize animations with the browser’s refresh rate.                | Medium | `js<br>requestAnimationFrame(() => {...});`                  |
| **Backend**  | **Use Caching**                                | Cache responses to reduce server load and response time.               | High   | Use `Redis`, `Memcached`, or HTTP caching.                   |
| **Backend**  | **Optimize Database Queries**                  | Use indexes and avoid `SELECT *` queries.                              | High   | `sql<br>SELECT id, name FROM users WHERE id = ?;`            |
| **Backend**  | **Reduce Payload Size**                        | Minimize the size of API responses.                                    | High   | Return only necessary fields in JSON.                        |
| **Backend**  | **Use Connection Pooling**                     | Maintain a pool of database connections to reduce connection overhead. | Medium | Configure `pg-pool` for PostgreSQL.                          |
| **Backend**  | **Use HTTP Keep-Alive**                        | Keep connections open to reduce handshake time.                        | Medium | Enable `keep-alive` on the server.                           |
| **Backend**  | **Avoid Synchronous Code in Request Handling** | Use async code to avoid blocking the event loop.                       | High   | `js<br>const data = await fetchData();`                      |
| **Backend**  | **Optimize Middleware and Routing**            | Minimize the number of middleware and simplify routing.                | Medium | Use `Express.js` with efficient routing.                     |
| **Backend**  | **Reduce Memory Leaks**                        | Avoid holding references to unused objects.                            | High   | Use `WeakMap` or `WeakSet` for temporary data.               |
| **Backend**  | **Use Load Balancing**                         | Distribute requests across multiple servers.                           | High   | Use `NGINX` or `HAProxy` for load balancing.                 |
| **Backend**  | **Enable Gzip/Brotli Compression**             | Compress API responses to reduce bandwidth.                            | High   | Configure server to enable Gzip/Brotli.                      |

## HTTP/2 and HTTP/3

### HTTP/2

| Feature                                           | Description                                                                          | Benefit                                                                | Example                                                             |
| ------------------------------------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Multiplexing**                                  | Multiple requests and responses can be sent simultaneously over a single connection. | Reduces latency and improves performance.                              | Load HTML, CSS, and JS files simultaneously over one connection.    |
| **Header Compression**                            | Compresses HTTP headers using HPACK.                                                 | Reduces bandwidth and speeds up request/response times.                | Large headers like `User-Agent` and `Cookie` are compressed.        |
| **Stream Prioritization**                         | Allows clients to specify the priority of different streams.                         | Ensures that critical resources load first.                            | Load CSS and JS before images.                                      |
| **Server Push**                                   | Server can push resources to the client before they are requested.                   | Reduces round trips and speeds up load times.                          | Server sends CSS and JS files along with the HTML.                  |
| **Binary Protocol**                               | Uses binary format instead of plain text.                                            | Faster parsing and reduced overhead.                                   | Headers and data frames are binary.                                 |
| **Single Connection**                             | All streams use a single connection.                                                 | Reduces the need for multiple TCP connections.                         | Browser keeps one persistent connection to the server.              |
| **Flow Control**                                  | Client and server can control the rate of data transfer.                             | Prevents congestion and improves performance.                          | Server adjusts data flow based on network conditions.               |
| **ALPN (Application-Layer Protocol Negotiation)** | Allows quick protocol negotiation between client and server.                         | Reduces latency during SSL/TLS handshake.                              | TLS handshake includes HTTP/2 negotiation.                          |
| **No Head-of-Line (HOL) Blocking**                | Streams are independent; one blocked stream doesn’t affect others.                   | Ensures that other resources are not delayed by a single slow request. | If a JS file is slow to load, other files still load.               |
| **Connection Coalescing**                         | Multiple domains can share the same connection if they resolve to the same IP.       | Reduces connection overhead.                                           | Requests to `example.com` and `cdn.example.com` use one connection. |

### HTTP/3

| Feature                                     | Description                                                                      | Benefit                                                                            | Example                                                      |
| ------------------------------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| **Uses QUIC Protocol**                      | Replaces TCP with UDP-based QUIC.                                                | Reduces connection establishment time and improves performance over poor networks. | Faster page loads on high-latency networks.                  |
| **Multiplexing**                            | Multiple streams over a single connection.                                       | No head-of-line blocking at the transport layer.                                   | HTML, CSS, and JS files load simultaneously.                 |
| **Connection Migration**                    | Maintains connections even if the IP address changes.                            | Ensures uninterrupted connection during network changes.                           | Switching from Wi-Fi to mobile data without reconnecting.    |
| **0-RTT (Zero Round Trip Time) Resumption** | Resumes a previous session without a full handshake.                             | Reduces connection establishment time.                                             | Page loads faster after an initial visit.                    |
| **Encryption by Default**                   | QUIC requires TLS 1.3 encryption.                                                | Ensures secure data transmission.                                                  | HTTPS is required for all HTTP/3 connections.                |
| **Packet Loss Recovery**                    | Independent streams mean that lost packets only affect the corresponding stream. | Improves reliability and reduces latency.                                          | A missing CSS file doesn't delay loading of other files.     |
| **Header Compression**                      | Compresses headers using QPACK (similar to HPACK).                               | Reduces bandwidth and improves request/response speed.                             | Large headers like `User-Agent` and `Cookie` are compressed. |
| **No Head-of-Line (HOL) Blocking**          | QUIC streams are independent at the transport level.                             | Avoids blocking caused by packet loss.                                             | A slow-loading image doesn’t block other resources.          |
| **Forward Error Correction (FEC)**          | Can recover from packet loss without retransmission.                             | Improves performance over poor networks.                                           | Streaming video remains smooth even with packet loss.        |
| **Simplified Handshake**                    | Combines transport and encryption handshake.                                     | Reduces handshake time and latency.                                                | Faster initial connection setup.                             |

## HTTP Headers

| Header Type          | Header                      | Description                                                           | Example                                                          |
| -------------------- | --------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **General Headers**  | `Cache-Control`             | Specifies caching rules for the response.                             | `Cache-Control: no-cache`                                        |
|                      | `Connection`                | Controls whether the connection stays open after the current request. | `Connection: keep-alive`                                         |
|                      | `Date`                      | The date and time the message was sent.                               | `Date: Wed, 21 Oct 2023 07:28:00 GMT`                            |
| **Request Headers**  | `Accept`                    | Informs the server about the type of content the client accepts.      | `Accept: text/html`                                              |
|                      | `Authorization`             | Provides credentials for authentication.                              | `Authorization: Bearer <token>`                                  |
|                      | `Host`                      | Specifies the host and port of the server.                            | `Host: example.com`                                              |
|                      | `User-Agent`                | Provides information about the client’s software.                     | `User-Agent: Mozilla/5.0`                                        |
|                      | `Referer`                   | Identifies the URL of the referring page.                             | `Referer: https://example.com`                                   |
| **Response Headers** | `Location`                  | Specifies the URL for redirection.                                    | `Location: https://example.com/new-page`                         |
|                      | `Server`                    | Describes the software used by the server.                            | `Server: Apache/2.4.1`                                           |
|                      | `Set-Cookie`                | Sends cookies from the server to the client.                          | `Set-Cookie: sessionId=abc123; HttpOnly`                         |
|                      | `WWW-Authenticate`          | Indicates the authentication method that should be used.              | `WWW-Authenticate: Basic`                                        |
| **Entity Headers**   | `Content-Type`              | Describes the media type of the response.                             | `Content-Type: application/json`                                 |
|                      | `Content-Length`            | Indicates the size of the response body in bytes.                     | `Content-Length: 1234`                                           |
|                      | `Last-Modified`             | Date and time when the resource was last modified.                    | `Last-Modified: Tue, 20 Jul 2023 10:00:00 GMT`                   |
| **Security Headers** | `Strict-Transport-Security` | Enforces HTTPS for future requests.                                   | `Strict-Transport-Security: max-age=31536000; includeSubDomains` |
|                      | `Content-Security-Policy`   | Restricts allowed sources for content.                                | `Content-Security-Policy: default-src 'self'`                    |
|                      | `X-Content-Type-Options`    | Prevents MIME type sniffing.                                          | `X-Content-Type-Options: nosniff`                                |
|                      | `X-Frame-Options`           | Prevents clickjacking attacks.                                        | `X-Frame-Options: DENY`                                          |
|                      | `X-XSS-Protection`          | Enables cross-site scripting (XSS) protection.                        | `X-XSS-Protection: 1; mode=block`                                |

## Web Workers Overview

| Feature                   | Description                                                                                | Example                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| **What is a Web Worker?** | A Web Worker allows JavaScript to run in the background without blocking the main thread.  | `const worker = new Worker('worker.js');`                    |
| **Creating a Worker**     | You create a worker using the `Worker` constructor and pass the script URL.                | `const worker = new Worker('worker.js');`                    |
| **Sending Messages**      | You use `postMessage()` to send messages from the main thread to the worker.               | `worker.postMessage('Hello Worker');`                        |
| **Receiving Messages**    | Use the `onmessage` event handler to handle messages in the worker.                        | `worker.onmessage = (event) => console.log(event.data);`     |
| **Terminating a Worker**  | You can stop a worker using the `terminate()` method.                                      | `worker.terminate();`                                        |
| **Importing Scripts**     | You can use `importScripts()` to import other scripts into a worker.                       | `importScripts('script1.js', 'script2.js');`                 |
| **Error Handling**        | Use the `onerror` event handler to catch errors in a worker.                               | `worker.onerror = (event) => console.error(event.message);`  |
| **Dedicated Worker**      | A worker that communicates with a single script.                                           | `const worker = new Worker('worker.js');`                    |
| **Shared Worker**         | A worker that can be accessed by multiple scripts from the same origin.                    | `const sharedWorker = new SharedWorker('shared-worker.js');` |
| **Worker Scope**          | Workers have access to the `self` object, but not to the DOM.                              | `self.onmessage = (e) => console.log(e.data);`               |
| **Transferrable Objects** | Use `postMessage()` with `Transferable` objects like `ArrayBuffer` to improve performance. | `worker.postMessage(arrayBuffer, [arrayBuffer]);`            |
| **Limitations**           | No access to the DOM, `window`, `document`, or `localStorage`.                             | N/A                                                          |

```js
// Create a new Worker
const worker = new Worker("worker.js");

// Send a message to the worker
worker.postMessage("Hello from main thread");

// Listen for messages from the worker
worker.onmessage = (event) => {
  console.log(`Message from worker: ${event.data}`);
};

// Handle errors
worker.onerror = (error) => {
  console.error(`Worker error: ${error.message}`);
};

// Terminate the worker
setTimeout(() => {
  worker.terminate();
  console.log("Worker terminated");
}, 5000);
```

```js
// Receive messages from the main thread
self.onmessage = (event) => {
  console.log(`Message from main thread: ${event.data}`);

  // Send a message back to the main thread
  self.postMessage(`Worker received: ${event.data}`);
};

// Example of using setInterval in a worker
setInterval(() => {
  self.postMessage("Worker is still working...");
}, 1000);
```

## Service Workers

| Feature                       | Description                                                                                                                  | Example                                                           |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **What is a Service Worker?** | A script that runs in the background, separate from the web page, handling caching, push notifications, and offline support. | `navigator.serviceWorker.register('/sw.js')`                      |
| **Registration**              | Use `navigator.serviceWorker.register()` to register a service worker.                                                       | `navigator.serviceWorker.register('/sw.js')`                      |
| **Lifecycle**                 | Service workers have an `install`, `activate`, and `fetch` lifecycle.                                                        | `self.addEventListener('install', event => { /* logic */ })`      |
| **Caching**                   | You can use the Cache API to store and serve assets.                                                                         | `caches.open('my-cache').then(cache => cache.add('/index.html'))` |
| **Fetching from Cache**       | Intercept network requests and serve from cache if available.                                                                | `event.respondWith(caches.match(event.request))`                  |
| **Background Sync**           | Service workers can sync data in the background when the network is available.                                               | `self.addEventListener('sync', event => { /* sync logic */ })`    |
| **Push Notifications**        | Service workers can receive and display push notifications.                                                                  | `self.addEventListener('push', event => { /* push logic */ })`    |
| **Message Passing**           | Communicate between the main thread and the service worker using `postMessage()`.                                            | `navigator.serviceWorker.controller.postMessage('Hello')`         |
| **Scope**                     | Controls which pages the service worker can intercept requests for.                                                          | `navigator.serviceWorker.register('/sw.js', { scope: '/' })`      |
| **Clients**                   | The `clients` API allows the service worker to communicate with all controlled pages.                                        | `self.clients.matchAll().then(clients => { /* logic */ })`        |
| **Limitations**               | No DOM access, only runs over HTTPS (except localhost).                                                                      | N/A                                                               |

```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker
    .register("/sw.js")
    .then((registration) => {
      console.log("Service Worker registered with scope:", registration.scope);
    })
    .catch((error) => {
      console.log("Service Worker registration failed:", error);
    });
}
```

```js
// Install event: Cache files
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("my-cache-v1").then((cache) => {
      return cache.addAll(["/", "/index.html", "/styles.css", "/script.js"]);
    })
  );
});

// Activate event: Clean up old caches
self.addEventListener("activate", (event) => {
  event.waitUntil(
    caches.keys().then((keys) => {
      return Promise.all(
        keys
          .filter((key) => key !== "my-cache-v1")
          .map((key) => caches.delete(key))
      );
    })
  );
});

// Fetch event: Serve files from cache or network
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});

// Push Notification Example
self.addEventListener("push", (event) => {
  const options = {
    body: event.data ? event.data.text() : "Default message",
    icon: "/icon.png",
  };
  event.waitUntil(
    self.registration.showNotification("Push Notification Title", options)
  );
});
```

## gRPC

| Feature                                 | Description                                                                                             | Example                                          |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **What is gRPC?**                       | gRPC (gRPC Remote Procedure Call) is a high-performance RPC framework developed by Google using HTTP/2. | `grpc.loadPackageDefinition()`                   |
| **Protocol**                            | Uses HTTP/2 for transport, Protobuf for serialization.                                                  | `protoLoader.loadSync()`                         |
| **IDL (Interface Definition Language)** | Defines service methods and messages using `.proto` files.                                              | `syntax = "proto3";`                             |
| **Serialization**                       | Uses Protocol Buffers (Protobuf) for compact and fast data serialization.                               | `message User { string name = 1; }`              |
| **Unary RPC**                           | Simple request-response model.                                                                          | `client.getUser({ id: 1 }, callback)`            |
| **Server Streaming RPC**                | Server sends multiple responses for a single request.                                                   | `client.getUserStream({ id: 1 })`                |
| **Client Streaming RPC**                | Client sends multiple messages to the server and gets a single response.                                | `call.write({ id: 1 })`                          |
| **Bidirectional Streaming RPC**         | Both client and server exchange multiple messages.                                                      | `call.on('data', response => { ... })`           |
| **Authentication**                      | Supports TLS encryption and token-based authentication.                                                 | `grpc.credentials.createSsl()`                   |
| **Load Balancing**                      | Built-in support for load balancing.                                                                    | Configured at server setup                       |
| **Deadlines and Timeouts**              | Clients can set timeouts for calls.                                                                     | `call = client.getUser({ id: 1 }, { deadline })` |
| **Interceptors**                        | Middleware to modify request/response behavior.                                                         | `grpc.interceptors.use(...)`                     |
| **Error Handling**                      | Status codes for specific gRPC errors.                                                                  | `grpc.status.NOT_FOUND`                          |
| **Language Support**                    | Supports multiple languages (JavaScript, Go, Python, etc.).                                             | `grpc.loadPackageDefinition()`                   |

## GraphQL

| Feature | Description | Example |
|---------|-------------|---------|
| **What is GraphQL?** | A query language for APIs, allowing clients to request exactly the data they need. | `query { user(id: 1) { name, email } }` |
| **Schema** | Defines the data structure and relationships in GraphQL. | `type User { id: ID!, name: String!, email: String! }` |
| **Query** | Requests specific fields on objects. | `query { user(id: 1) { name, email } }` |
| **Mutation** | Modifies data on the server. | `mutation { createUser(name: "John") { id, name } }` |
| **Subscription** | Allows real-time updates from the server. | `subscription { onUserAdded { id, name } }` |
| **Resolvers** | Functions that resolve queries and return data. | `const resolvers = { Query: { user: () => {...} } }` |
| **Type System** | Strongly typed system with scalar and object types. | `type User { id: ID!, name: String! }` |
| **Directives** | Special instructions for modifying query behavior. | `@skip, @include` |
| **Fragments** | Reusable units of fields in queries. | `fragment UserFields on User { id, name }` |
| **Interfaces** | Abstract types used to define common fields for objects. | `interface Person { id: ID!, name: String! }` |
| **Unions** | A type that can represent multiple types. | `union SearchResult = User | Post` |
| **Input Types** | Special types used for passing complex objects into queries or mutations. | `input UserInput { name: String! }` |
| **Introspection** | Ability to query the schema itself for metadata. | `__schema { types { name } }` |
| **Authentication** | Tokens or API keys used to secure queries. | `Authorization: Bearer <token>` |
| **Batching and Caching** | Reduces network requests by combining multiple queries. | `apollo-client` |
| **Error Handling** | Structured error responses from the server. | `{ errors: [{ message, locations, path }] }` |
