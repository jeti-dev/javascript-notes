

## 🟢 Beginner NestJS Topics

| Topic             | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| Modules           | Organize application structure into cohesive blocks.                        |
| Controllers       | Handle incoming requests and return responses.                              |
| Providers         | Services or classes that can be injected as dependencies.                   |
| Dependency Injection | NestJS's built-in DI system for managing class dependencies.             |
| Services          | Classes with business logic, injected into controllers.                     |
| Routing           | Define paths for controllers using decorators like `@Get()` or `@Post()`.   |
| CLI               | The Nest CLI helps generate and manage project files.                       |
| Lifecycle Hooks   | Hooks like `OnInit` and `OnDestroy` for handling component lifecycle.       |
| DTOs              | Define shapes of incoming data using TypeScript classes.                    |
| Pipes             | Transform and validate input data.                                          |

---

## 🟡 Intermediate NestJS Topics

| Topic             | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| Middleware        | Functions that run before request reaches the controller.                   |
| Guards            | Used for authorization and route protection.                               |
| Interceptors      | Logic that can run before/after request handling (e.g., logging, caching).  |
| Exception Filters | Handle and customize errors thrown during request processing.               |
| Custom Decorators | Create reusable decorators for parameters or class logic.                   |
| Async Providers   | Define providers that need to be created asynchronously.                    |
| Request Lifecycle | Understanding how Nest handles a request from start to finish.              |
| ConfigModule      | Load and manage environment variables/config settings.                      |
| Global Modules    | Modules made available app-wide without importing in each module.           |
| Param Decorators  | Extract specific parameters from routes, like `@Param`, `@Query`.           |

---

## 🔴 Advanced NestJS Topics

| Topic             | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| Microservices     | Build scalable systems using transport layers like TCP, Redis, NATS.        |
| GraphQL Integration | Integrate NestJS with GraphQL using Apollo or Mercurius.                 |
| WebSockets        | Real-time communication using the `@WebSocketGateway()` module.             |
| Custom Pipes/Guards/Interceptors | Build custom logic for advanced control flow.              |
| Caching Strategies | Use cache interceptors or Redis for response caching.                      |
| Task Scheduling   | Use packages like `@nestjs/schedule` to run cron jobs.                      |
| CQRS Pattern      | Apply the Command Query Responsibility Segregation for large apps.          |
| Domain Driven Design (DDD) | Structure your app into meaningful domain modules.                |
| Testing (E2E & Mocks) | Advanced testing strategies using Jest and Supertest.                   |
| Dynamic Modules   | Modules that are configured dynamically with different options.             |
