# Express.js Cheatsheet

## 1. App Initialization

---

API Description Example

---

`express()` Creates an Express `const app = express()`
application

`app.listen()` Starts HTTP server `app.listen(3000)`

`app.set()` Set application `app.set('trust proxy', true)`
settings

`app.get()` Get application setting `app.get('env')`

`app.enable()` Enable setting `app.enable('trust proxy')`

`app.disable()` Disable setting `app.disable('x-powered-by')`

---

```ts
import express from "express";

const app = express();

app.listen(3000, () => {
  console.log("Server running");
});
```

---

# 2. Routing

---

Method Purpose Example

---

`app.get()` Handle GET request `app.get('/users', handler)`

`app.post()` Handle POST request `app.post('/users', handler)`

`app.put()` Replace resource `app.put('/users/:id', handler)`

`app.patch()` Partial update `app.patch('/users/:id', handler)`

`app.delete()` Delete resource `app.delete('/users/:id', handler)`

`app.all()` Handle all HTTP methods `app.all('/endpoint', handler)`

---

```ts
app.get("/users/:id", (req, res) => {
  res.json({ id: req.params.id });
});
```

---

# 3. Router (Modular Routing)

API Description

---

`express.Router()` Creates modular route handlers

`router.use()` Attach middleware to router

`router.get()` Define route

`router.param()` Parameter middleware

```ts
const router = express.Router();

router.get("/users", getUsers);

app.use("/api", router);
```

---

# 4. Request Object (`req`)

Property Description

---

`req.params` Route parameters

`req.query` Query string

`req.body` Parsed body

`req.headers` Request headers

`req.method` HTTP method

`req.url` Request URL

`req.ip` Client IP

`req.cookies` Cookies (via middleware)

---

# 5. Response Object (`res`)

API Description Example

---

`res.send()` Send response `res.send('ok')`

`res.json()` Send JSON `res.json({ success: true })`

`res.status()` Set status code `res.status(404)`

`res.redirect()` Redirect request `res.redirect('/login')`

`res.sendFile()` Send file `res.sendFile(path)`

`res.download()` File download `res.download(file)`

`res.set()` Set header `res.set('Cache-Control', 'no-store')`

```ts
res.status(200).json({ message: "success" });
```

---

# 6. Middleware

Type Description

---

Application middleware Attached to app
Router middleware Attached to router
Error middleware Handles errors
Built-in middleware Provided by Express

Signature:

```ts
(req, res, next) => {};
```

Example:

```ts
app.use((req, res, next) => {
  console.log(req.method, req.url);
  next();
});
```

---

# 7. Built-in Middleware

Middleware Purpose

---

`express.json()` Parse JSON body

`express.urlencoded()` Parse form data

`express.static()` Serve static files

`express.raw()` Parse raw body

`express.text()` Parse text body

```ts
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static("public"));
```

---

# 8. Error Handling

Error middleware has **4 parameters**.

Signature Purpose

---

`(err, req, res, next)` Handle application errors

```ts
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ error: "Internal Server Error" });
});
```

---

# 9. Async Handlers Pattern

Express does **not automatically catch async errors**.

```ts
const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```

Usage:

```ts
app.get(
  "/users",
  asyncHandler(async (req, res) => {
    const users = await db.getUsers();
    res.json(users);
  }),
);
```

---

# 10. Route Parameters

Pattern Example

---

Named param `/users/:id`

Optional param `/users/:id?`

Regex `/users/:id(\\d+)`

Multiple `/users/:userId/posts/:postId`

---

# 11. Static Files

```ts
app.use(express.static("public"));
```

Example structure:

    public/
      css/
      js/
      images/

---

# 12. Application Settings

Setting Purpose

---

`env` Environment

`trust proxy` Reverse proxy support

`case sensitive routing` Case sensitivity

`strict routing` Trailing slash sensitivity

`x-powered-by` Remove Express header

```ts
app.disable("x-powered-by");
app.set("trust proxy", true);
```

---

# 13. Common Production Middleware

Package Purpose

---

`helmet` Security headers

`cors` Cross-origin requests

`morgan` HTTP logging

`compression` Gzip compression

`cookie-parser` Parse cookies

`express-rate-limit` Rate limiting

```ts
import helmet from "helmet";
import cors from "cors";

app.use(helmet());
app.use(cors());
```

---

# 14. Mounting Middleware

Pattern Example

---

Global `app.use(middleware)`

Path-specific `app.use('/api', middleware)`

Router `router.use(middleware)`

```ts
app.use("/api", apiRouter);
```

---

# 15. Request Lifecycle

    Request
       ↓
    Application middleware
       ↓
    Route middleware
       ↓
    Route handler
       ↓
    Response
       ↓
    Error middleware (if needed)

---

# 16. Recommended Project Structure

    src/
      controllers/
      routes/
      services/
      middleware/
      models/
      utils/
      app.ts
      server.ts

---

# 17. Minimal Production Setup

```ts
import express from "express";
import helmet from "helmet";
import cors from "cors";

const app = express();

app.use(helmet());
app.use(cors());
app.use(express.json());

app.get("/health", (req, res) => {
  res.json({ status: "ok" });
});

app.listen(3000);
```
