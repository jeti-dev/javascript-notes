

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

## Notes of the documentation
### Controllers
- express or fastify is under the hood
- objects and arrays are automatically serialized on return from a controller method
    - 200 for GET, 201 for POST
    - you can use the underlying platform res with @Res - otherwise you don't need to use @Res!
    - set passthrough: use the underlying @Res + the default Nest behavior
- instead or @Req we can use @Param, @Body, @Query, @Headers
- change status code: @HttpCode on controller method
- custom res header: @Header
- redirect: @Redirect
- from an async controller method, you must return a Promise -> just set the return type and return a simple value, no need for creating a promise

### Providers
- @Optional can indicate a dependency is optional
- injection type: constructor, property (to avoid passing dependency to the parent class)

### Modules
- imports: modules which export providers this module uses
- exports: providers that I export for other modules to use
- a module can inject providers e.g. to configure them
- @Global on the module: makes the modul automatically global
- dynamic module: config a module at runtime

### Middleware
- called before the route handler
- @Injectable
- class based (NestMiddleware interface) or function
- apply it to a module (NestModule interface) and config to which routes to apply
- global middleware: app.use() after create

### Exception filters
- catches unhandled exceptions and return a user friendly message
- use the HttpException class with statusCode and message
    - there is a property called cause that is not returned but can be used for logging
- custom exception filter: implement ExceptionFilter interface + add @Catch(HttpException)
- bind a filter: @UseFilters(MyFilter)
- if you want to only customize the base filter a bit: extend it and call super.catch() in the end

### Pipes
- PipeTransform interface
- transform data, validate data
- e.g. @Param('id', ParseIntPipe) but we could pass an instance that we configured
- pipes throw an exception on receiving null or undefined -> use the DefaultValuePipe

### Guards
- @Injectable + CanActivate interface
- authorization
- executed after all middleware but before any interceptor or pipe
- setting custom metadata e.g. roles: const Roles = Reflector.createDecorator<string[]>() (or @SetMetadata()) then use it like @Roles(['admin'])
    - get the metadata value in the guard: this.reflector.get(Roles, context.getHandler())

### Interceptors
- @Injectable + NestInterceptor interface
- can run code before or after the route handler
- usage: @UseInterceptors(LoggingInterceptor)

### Custom decorators
- e.g. get the User object as a controller method parameter from the req object
- applyDecorators can group decorators into 1

### Custom providers
- {
    provide: MyClass/string/symbol/enum,
    useValue: whatever,
    useClass: whatever,
    useFactory: factory function to create stuff dynamically,
    useExisting: whatever2 -> aliasing an existing dependency
}
- the token or the whole object can be exported from the module to be used by other modules

### Async providers
- use an async function for useFactory

### Dynamic modules
- e.g. you have a config module that should behave differently in different environments -> you need to configure the module somehow
- MyModule.register(config): it must return a DynamicModule object which has an extra property called module which is the module class (MyModule)
    - then we add this config object as a custom provider to the module's providers array so it can be injected into our services
- register: you would configure the same module multiple times in a different way in the app
- forRoot: you would configure the module once and reuse that module
- forFeature: you want to use the config set by a forRoot but want to customize it a bit
- these have async versions too
- ConfigurableModuleBuilder: helps with writing configurable modules

### Injection scopes
- by default everything is shared across incoming requests -> we might not want that every time
- scopes:
    - default: one instance in the whole app
    - request: one instance per request
    - transient: create a new instance when it's injected
- usage: "scope" config in @Injectable or "scope" in the custom provider object
- @INQUIRER: inject the class that our @Injectable was injected into -> e.g. log the class name which is using our logger service
- durable providers: when we have a multi tenant app (e.g. mulitiple companies) we don't want to request scope (e.g. beacause of configs) the everything -> create durable providers that keep tenant specific sub trees

### Circular dependency
-  @Inject(forwardRef(() => ServiceA)) and do the same in ServiceA for ServiceB
- OR in the @Module: imports: [forwardRef(() => CatsModule)] and do the same for the other module

### Module reference
- you can injest ModuleRef in the constructor then you can get stuff from the module e.g. the service objects
- set {strict: false} to get global providers
- moduleRef.resolve(MyService): get transient or request scoped service
    - you can get the same instance every time from resolve() if you set the contextId
    - to get a request scoped provider, we need to register it to a context first
    - we can use the ContextIdFactory.getByRequest(req) to get the contextId
- moduleRef.create(MyService): create dynamicaly a provider which wast not in the providers array of the module