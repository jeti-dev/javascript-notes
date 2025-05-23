---
title: Web
layout: default
---

# Web

- `stopPropagation` vs `stopImmediatePropagation`: the second one also disables the other events of the same type on the target
- `requestAnimationFrame`: it is a browser API that tells the browser to call a specified function before the next repaint

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


## Rendering Strategies Overview

- hydration: server side rendering or static rendering the page and send it to the user then switch to CSR
- incremental static regeneration: you have static sites but the server re-creates them based on some rules
- partial hydration: e.g. render the footer only when the user scrolls there
- streaming rendering: render the server-side rendered parts on the fly, concurrently

| Strategy | Description | Pros | Cons | Example |
|----------|-------------|------|------|---------|
| **Client-Side Rendering (CSR)** | Renders the page on the client side using JavaScript. | - Fast initial load after first request<br>- Lower server load | - Slower initial load<br>- Poor SEO (without hydration) | React with `useEffect()` |
| **Server-Side Rendering (SSR)** | Renders the page on the server and sends HTML to the client. | - Faster initial load<br>- Better SEO<br>- Works without JavaScript | - Increased server load<br>- Slower TTFB (Time to First Byte) | Next.js `getServerSideProps` |
| **Static Site Generation (SSG)** | Pre-renders pages at build time. | - Fast load times<br>- Good SEO<br>- Cached for quick delivery | - Stale data unless revalidated<br>- Long build times with large content | Next.js `getStaticProps` |
| **Incremental Static Regeneration (ISR)** | Rebuilds static pages in the background when needed. | - Fast load times<br>- Keeps content fresh<br>- Reduced build times | - Cache invalidation complexity | Next.js `revalidate: 10` |
| **Progressive Hydration** | Loads and renders critical parts of the page first, then hydrates others. | - Faster perceived load<br>- Better user experience | - Complex to implement<br>- Potential race conditions | React.lazy() + Suspense |
| **Partial Rendering** | Renders only specific parts of a page instead of the whole page. | - Faster load times<br>- Lower memory usage | - State inconsistency<br>- More complex logic | React `lazy()` and `Suspense` |
| **Streaming Rendering** | Renders and sends HTML in chunks while it's still generating. | - Faster initial content display<br>- Improved UX | - Requires HTTP/2 or higher<br>- Complexity | `ReactDOMServer.renderToNodeStream()` |
| **Hybrid Rendering** | Combines CSR, SSR, and SSG as needed. | - Flexibility<br>- Optimized performance | - Increased implementation complexity | Next.js mixed strategy |


## 🍪 Cookies Overview

| **Type** | **Description** | **Example Use Case** |
|---------|----------------|---------------------|
| **Session Cookies** | Temporary cookies deleted when the browser is closed. | User login session |
| **Persistent Cookies** | Stored on the user's device for a set period. | "Remember me" feature |
| **Secure Cookies** | Sent only over HTTPS to prevent interception. | Storing session tokens securely |
| **HttpOnly Cookies** | Accessible only by the server (not readable by JavaScript). | Prevents XSS attacks |
| **SameSite Cookies** | Restrict cross-site request behavior. | Protects against CSRF attacks |
| **First-Party Cookies** | Created by the domain the user is visiting. | User authentication |
| **Third-Party Cookies** | Created by external domains (like ads). | Tracking and advertising |

---

### 📌 Common Attributes

| **Attribute** | **Description** | **Example** |
|-------------|----------------|-------------|
| **name** | Name of the cookie. | `session_id` |
| **value** | Value stored in the cookie. | `abc123` |
| **domain** | Domain where the cookie is available. | `.example.com` |
| **path** | URL path where the cookie is accessible. | `/account` |
| **expires** | Expiration date of the cookie. | `Tue, 19 Jan 2038 03:14:07 GMT` |
| **max-age** | Lifetime in seconds from creation. | `3600` (1 hour) |
| **secure** | Only sent over HTTPS. | `Secure` |
| **httponly** | Inaccessible to JavaScript (`document.cookie`). | `HttpOnly` |
| **samesite** | Controls cross-site behavior (`Lax`, `Strict`, `None`). | `SameSite=Lax` |

---

### 🌐 HTTP Header Example

| **Header** | **Example** |
|------------|-------------|
| **Set-Cookie** | `Set-Cookie: session_id=abc123; Path=/; HttpOnly; Secure; SameSite=Strict` |

---

### 🔒 SameSite Values

| **Value** | **Description** | **Example Use Case** |
|----------|----------------|----------------------|
| **Strict** | Only sent in a first-party context. | Highest security but may break some cross-site flows. |
| **Lax** | Sent for top-level navigation only. | Good balance between security and usability. |
| **None** | Sent in all contexts but requires `Secure`. | For third-party cookies. |

---

### ⚠️ Common Security Risks

| **Risk** | **Description** | **Solution** |
|---------|----------------|-------------|
| **XSS (Cross-Site Scripting)** | Malicious scripts accessing cookies. | Use `HttpOnly` and `Secure` attributes. |
| **CSRF (Cross-Site Request Forgery)** | Unauthorized actions using cookies. | Use `SameSite=Lax` or `Strict`. |
| **Session Hijacking** | Stolen session cookies. | Use `Secure`, `HttpOnly`, and proper session management. |


## Progressive Web Apps (PWA)

| **Category** | **Description** |
|-------------|-----------------|
| **Core Principles** | Responsive, Reliable, Installable, Engaging, Secure |
| **Manifest Properties** | `name`, `short_name`, `start_url`, `display`, `background_color`, `theme_color`, `icons` |
| **Display Modes** | `browser`, `minimal-ui`, `standalone`, `fullscreen` |
| **Caching Strategies** | **Cache First** – Serve from cache, fallback to network<br>**Network First** – Try network first, fallback to cache<br>**Stale While Revalidate** – Serve from cache, update in background<br>**Cache Only** – Serve from cache only<br>**Network Only** – Always request from network |
| **Service Worker Lifecycle** | `install` – Register service worker<br>`activate` – Clean up old caches<br>`fetch` – Handle network requests |
| **Offline Handling** | **Service Workers** – Handle caching and background syncing<br>**Cache Storage** – Save files and assets for offline use<br>**IndexedDB** – Store structured data locally<br>**Background Sync** – Sync data when the connection is restored |
| **Security Requirements** | HTTPS for service workers and installation<br>Explicit permissions for notifications, geolocation<br>Content Security Policy (CSP) to prevent XSS attacks |
| **App Install Prompt** | Trigger installation prompt using `beforeinstallprompt` event |
| **Best Practices** | Optimize assets, Use lazy loading, Responsive design, Secure HTTPS, Offline fallback UI |


## Time Complexity of Common Algorithms


| **Algorithm Type** | **Best Case** | **Average Case** | **Worst Case** | **Description** | **Example** |
|-------------------|--------------|------------------|----------------|----------------|-------------|
| **Constant Time (O(1))** | O(1) | O(1) | O(1) | The operation takes the same amount of time regardless of input size | `arr.push(1)` → Adding an element to the end of an array |
| **Logarithmic Time (O(log n))** | O(1) | O(log n) | O(log n) | The number of operations increases logarithmically with the input size | `arr.indexOf(value)` → Binary search (if sorted) |
| **Linear Time (O(n))** | O(n) | O(n) | O(n) | The number of operations increases linearly with input size | `arr.forEach(el => console.log(el))` → Iterating over an array |
| **Linearithmic Time (O(n log n))** | O(n log n) | O(n log n) | O(n log n) | The number of operations increases with input size times a logarithmic factor | `arr.sort()` → Sorting an array (uses quicksort or mergesort) |
| **Quadratic Time (O(n²))** | O(n) | O(n²) | O(n²) | The number of operations increases with the square of the input size | Nested loops: `for (let i of arr) { for (let j of arr) {} }` |
| **Cubic Time (O(n³))** | O(n²) | O(n³) | O(n³) | The number of operations increases with the cube of the input size | Triple nested loops: `for (let i of arr) { for (let j of arr) { for (let k of arr) {} } }` |
| **Exponential Time (O(2ⁿ))** | O(1) | O(2ⁿ) | O(2ⁿ) | The number of operations doubles with each additional input size | Recursive Fibonacci: `const fib = n => n < 2 ? n : fib(n - 1) + fib(n - 2)` |
| **Factorial Time (O(n!))** | O(1) | O(n!) | O(n!) | The number of operations increases with the factorial of the input size | Generating permutations using recursion |

## Local and session storage

| Feature                   | localStorage                                               | sessionStorage                                             |
|-------------------------- |------------------------------------------------------------|----------------------------------------------------------|
| **Scope**                 | Accessible across all windows and tabs from the same origin | Accessible only within the same tab                      |
| **Persistence**           | Data persists even after closing and reopening the browser  | Data is lost when the tab or window is closed             |
| **Storage Limit**         | Approximately **5-10MB** depending on the browser           | Approximately **5-10MB** depending on the browser         |
| **Data Type**             | Only stores strings (must manually serialize objects)        | Only stores strings (must manually serialize objects)      |
| **Access**                | Can be accessed using `localStorage.getItem()` and `localStorage.setItem()` | Can be accessed using `sessionStorage.getItem()` and `sessionStorage.setItem()` |
| **Security**              | Stored data is accessible from any window or tab under the same origin | Data is isolated to a single tab                          |
| **Example Usage**         | Saving user preferences, tokens, or theme settings          | Temporary session data like form input or state management within a session |
| **Supported Events**      | `storage` event triggers when data is changed in other tabs | `storage` event does **not** trigger for sessionStorage   |

## IndexedDB

| Feature                            | Description                                                                                      |
|------------------------------------|--------------------------------------------------------------------------------------------------|
| **Scope**                          | Accessible across multiple windows, tabs, and worker threads within the same origin               |
| **Persistence**                    | Data persists even after the browser is closed                                                    |
| **Data Type**                      | Can store any type of data, including objects, arrays, and files (binary data)                   |
| **Structure**                      | Uses object stores (similar to tables) and keys to store data                                     |
| **Transactions**                   | Supports atomic transactions to ensure data consistency                                           |
| **Indexing**                       | Supports creating indexes to enable fast data lookup                                              |
| **Performance**                    | Designed for handling large amounts of structured data efficiently                                |
| **API Type**                       | Asynchronous API based on Promises                                                                |
| **Access Method**                  | Uses `indexedDB.open()` to create or open a database                                              |
| **Example Usage**                  | Storing cached data, offline-first web apps, and structured data management                        |
| **Limitations**                    | Not supported in all browsers; handling complex transactions can be difficult                      |
| **Events**                         | Supports `onsuccess`, `onerror`, `onupgradeneeded`, and other lifecycle events                   |
| **Versioning**                     | Each database is versioned; upgrades require handling `onupgradeneeded` event                     |

``` ts
const request = indexedDB.open('myDatabase', 1);

request.onupgradeneeded = (event) => {
  const db = request.result;
  if (!db.objectStoreNames.contains('users')) {
    db.createObjectStore('users', { keyPath: 'id' });
  }
};

request.onsuccess = (event) => {
  console.log('Database opened successfully');
};

request.onerror = (event) => {
  console.error('Error opening database', event);
};
```

``` ts
const request = indexedDB.open('myDatabase', 1);

request.onsuccess = (event) => {
  const db = request.result;
  const transaction = db.transaction('users', 'readwrite');
  const store = transaction.objectStore('users');

  const user = { id: 1, name: 'John Doe', age: 30 };
  store.add(user);

  transaction.oncomplete = () => {
    console.log('User added');
  };

  transaction.onerror = (event) => {
    console.error('Error adding user', event);
  };
};

```

## Offline

| **Feature** | **Description** | **Example** |
|------------|----------------|------------|
| **Service Workers** | Background script that acts as a proxy between the browser and network, allowing caching and offline access. | Use `self.addEventListener('install', (event) => {...})` to cache resources. |
| **Cache API** | Provides a way to store network responses and assets for offline use. | `caches.open('my-cache').then(cache => cache.addAll(['/index.html', '/styles.css']));` |
| **IndexedDB** | Client-side NoSQL database for structured data storage, used for offline data storage and syncing. | Create a database using `indexedDB.open('myDB', 1)`. |
| **LocalStorage** | Stores key-value pairs in the browser, data persists even after the browser is closed. | `localStorage.setItem('key', 'value');` |
| **SessionStorage** | Similar to LocalStorage but persists only for the duration of the session. | `sessionStorage.setItem('key', 'value');` |
| **Background Sync** | Allows the app to retry failed network requests when the connection is restored. | `registration.sync.register('mySyncTag');` |
| **Web Manifest** | JSON file that provides metadata about the app (e.g., name, icons, start URL). | `{"name": "My App", "start_url": "/index.html"}` |
| **Network Information API** | Allows the app to detect network status and adjust functionality accordingly. | `navigator.onLine` returns `true` or `false`. |
| **Progressive Web Apps (PWAs)** | Apps that use Service Workers and Web Manifest to work offline and provide a native app-like experience. | `install` event in the Service Worker to cache assets. |
| **AppCache** *(Deprecated)* | Legacy API that allowed caching of resources for offline use. | No longer supported in modern browsers. |

## Cache API

| **Method** | **Description** | **Example** |
|------------|----------------|------------|
| **caches.open(name)** | Opens a new cache or returns an existing one. | `caches.open('my-cache').then(cache => {...})` |
| **cache.add(request)** | Fetches and adds a response to the cache. | `cache.add('/index.html');` |
| **cache.addAll(requests)** | Fetches and adds multiple responses to the cache. | `cache.addAll(['/index.html', '/styles.css']);` |
| **cache.put(request, response)** | Adds a custom response to the cache. | `cache.put('/data', new Response('Custom Data'));` |
| **cache.match(request)** | Returns a cached response if it exists. | `cache.match('/index.html').then(response => {...})` |
| **cache.delete(request)** | Deletes a cached response. | `cache.delete('/index.html');` |
| **caches.keys()** | Returns a list of cache names. | `caches.keys().then(keys => console.log(keys));` |
| **caches.match(request)** | Returns a cached response from any cache. | `caches.match('/index.html').then(response => {...})` |
| **caches.delete(name)** | Deletes a specific cache. | `caches.delete('my-cache');` |

---

## Web Manifest

| **Property** | **Description** | **Example** |
|-------------|----------------|-------------|
| **name** | Name of the app. | `"name": "My App"` |
| **short_name** | Short name displayed on the home screen. | `"short_name": "App"` |
| **start_url** | URL to load when the app is launched. | `"start_url": "/index.html"` |
| **display** | Controls how the app is displayed (fullscreen, standalone, etc.). | `"display": "standalone"` |
| **icons** | Array of icon objects for different sizes and resolutions. | `"icons": [{"src": "/icon.png", "sizes": "192x192", "type": "image/png"}]` |
| **theme_color** | Color of the browser UI elements (e.g., address bar). | `"theme_color": "#ffffff"` |
| **background_color** | Background color of the splash screen. | `"background_color": "#000000"` |
| **orientation** | Preferred screen orientation. | `"orientation": "portrait"` |
| **scope** | Defines the navigation scope of the app. | `"scope": "/app/"` |
| **description** | Short description of the app. | `"description": "This is a demo app."` |

## WebRTC API

| **Component** | **Description** | **Example** |
|--------------|----------------|-------------|
| **RTCPeerConnection** | Handles the connection between peers. Responsible for negotiating and managing the peer-to-peer connection. | `const pc = new RTCPeerConnection();` |
| **RTCSessionDescription** | Represents the local or remote description of a connection. Contains SDP (Session Description Protocol) data. | `const desc = new RTCSessionDescription(offer);` |
| **RTCIceCandidate** | Represents a candidate for network connectivity during connection establishment. | `const candidate = new RTCIceCandidate({...});` |
| **getUserMedia** | Captures audio and video from the user’s device. Returns a `MediaStream` object. | `navigator.mediaDevices.getUserMedia({ video: true, audio: true });` |
| **addTrack** | Adds a media track to the peer connection. | `pc.addTrack(track, stream);` |
| **onicecandidate** | Event handler triggered when an ICE candidate is available. | `pc.onicecandidate = event => {...};` |
| **ontrack** | Event handler triggered when a new media track is received. | `pc.ontrack = event => {...};` |
| **createOffer** | Creates an SDP offer to start a connection. | `const offer = await pc.createOffer();` |
| **setLocalDescription** | Sets the local SDP description. | `await pc.setLocalDescription(offer);` |
| **setRemoteDescription** | Sets the remote SDP description. | `await pc.setRemoteDescription(answer);` |
| **createAnswer** | Creates an SDP answer to accept a connection. | `const answer = await pc.createAnswer();` |
| **close** | Closes the connection and releases resources. | `pc.close();` |


## WebSocket Overview

| Feature | Description |
|---------|-------------|
| **Protocol** | Follows the `ws://` or `wss://` protocol (WebSocket Secure). |
| **Full Duplex** | Allows bidirectional communication between client and server simultaneously. |
| **Persistent Connection** | Keeps a single connection open for real-time data exchange. |
| **Event-Driven** | Uses events like `onopen`, `onmessage`, `onclose`, and `onerror` to handle connection states. |
| **Low Latency** | Lower overhead compared to HTTP; ideal for real-time applications like chat and gaming. |
| **Binary and Text Support** | Can send and receive both text and binary data (e.g., JSON, ArrayBuffer). |
| **Security** | `wss://` uses TLS encryption for secure communication. |
| **API Methods** | - `WebSocket.send()` – Send data to server.<br>- `WebSocket.close()` – Close the connection.<br>- `WebSocket.readyState` – Connection status. |
| **Events** | - `onopen` – Triggered when connection opens.<br>- `onmessage` – Triggered when message is received.<br>- `onclose` – Triggered when connection closes.<br>- `onerror` – Triggered on connection error. |
| **Use Cases** | - Chat applications<br>- Online gaming<br>- Real-time stock updates<br>- Live streaming<br>- IoT device communication |

## Browser Event Phases

| Phase | Description | Order |
|-------|-------------|-------|
| **Capturing Phase** | The event starts at the root and travels down to the target element. | First |
| **Target Phase** | The event reaches the target element and triggers the event listener on the target. | Second |
| **Bubbling Phase** | The event travels back up the DOM tree from the target element to the root. | Third |
| **Use Cases** | - Use `addEventListener(type, listener, **true**)` to capture events in the capturing phase.<br>- Use `addEventListener(type, listener, **false**)` to listen in the bubbling phase.<br>- Not all events bubble (e.g., `focus`, `blur`, `mouseenter`, `mouseleave`). | |

## multipart/form-data vs application/x-www-form-urlencoded

| Feature | `multipart/form-data` | `application/x-www-form-urlencoded` |
|---------|-----------------------|-------------------------------------|
| **Definition** | Used for sending files and binary data along with form fields. | Used for sending key-value pairs in the request body. |
| **Encoding** | Data is divided into parts using boundaries. | Data is encoded as `key=value&key=value`. |
| **Binary Data Support** | ✅ Yes (can send files, images, etc.) | ❌ No (only text data) |
| **Text Data Support** | ✅ Yes | ✅ Yes |
| **File Uploads** | ✅ Yes (supports file uploads) | ❌ No (does not support files) |
| **Request Size** | Larger due to boundary overhead. | Smaller due to simple format. |
| **Complexity** | More complex to parse on the server. | Simple to parse. |
| **Use Case** | File uploads, mixed binary and text data submission. | Simple form data submission, login forms, etc. |
| **Example** | `enctype="multipart/form-data"` | `enctype="application/x-www-form-urlencoded"` |

## JSONP
It is used to bypass the same-origin policy when making HTTP requests from the browser to a different domain. It works by loading data through a script tag instead of using XMLHttpRequest or fetch, since script tags are not restricted by the same-origin policy.
- The client creates a script tag dynamically and sets the src attribute to a URL on the external server.
- The URL includes a query parameter specifying a callback function.
- The server responds with JavaScript code that calls the specified callback function with the data as an argument.
- The browser executes the response as a script, which triggers the callback and processes the data.