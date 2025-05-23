---
title: NestJs
layout: default
---


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
| Lifecycle Hooks   | Hooks like `onModuleInit` and `onModuleDestroy` for handling component lifecycle.       |
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
- you can inject ModuleRef in the constructor then you can get stuff from the module e.g. the service objects
- set {strict: false} to get global providers
- moduleRef.resolve(MyService): get transient or request scoped service
    - you can get the same instance every time from resolve() if you set the contextId
    - to get a request scoped provider, we need to register it to a context first
    - we can use the ContextIdFactory.getByRequest(req) to get the contextId
- moduleRef.create(MyService): create dynamicaly a provider which was not in the providers array of the module

### Lazy loading modules
- by default modules are eager loaded -> but it can be a problem in serverless apps where cold start is crucial
    - if the startup time is not crucial, this feature is not really useful
- LazyModuleLoader can be injected then its load() method can load a module -> moduleRef.get(MyService) can be used to get a provider from the lazy loaded module
- a lazy loaded module can't be global -> it doesn't make sense

### Execution context
- contexts e.g. http, microservice, websocket, rpc, graphql
    - host.getType() returns this value
- ArgumentsHost: methods for getting the arguments passed to the handler
    - e.g. when using express it contains [request, response, next]
    - host.getArgs(), host.getArgByIndex()
    - switchToRpc(), swtichToHttp(), etc. returns the associated AegumentsHost e.g. HttpArgumentsHost then you can use e.g. ctx.getRequest<MyReqType>()
- ExecutionContext extends ArgumentsHost
    -    getClass(), getHandler() refers to the method and controller which is going to be called -> we can get metadata we set on these
    - Reflector: can be used to get the metadata       
        - this.reflector.get(Roles, context.getHandler())
        - this.reflector.get(Roles, context.getClass())

### Lifecycle events
- steps:
    - onModuleInit() (can be async): Called once the host module's dependencies have been resolved.
    - onApplicationBootstrap() (can be async): Called once all modules have been initialized, but before listening for connections.
    - onModuleDestroy(): Called after a termination signal (e.g., SIGTERM) has been received.
    - beforeApplicationShutdown(): Called after all onModuleDestroy() handlers have completed (Promises resolved or rejected);
    once complete (Promises resolved or rejected), all existing connections will be closed (app.close() called).
    - onApplicationShutdown(): Called after connections close (app.close() resolves).
- onModuleDestroy, beforeApplicationShutdown, onApplicationShutdown: if you're not calling app.close() explicitly, you must opt-in to make them work with system signals such as SIGTERM
    - we must also enable them: enableShutdownHooks()
- they are interfaces so we need to implement in our controller, provider or module

### Discovery service
- we can get providers, controllers, metadata dynamically runtime
- mostly for writing extensible frameworks, plugins etc
- DiscoveryModule + DiscoveryService

### Platform agnosticism
- express vs fastify, http vs microservice vs websocket etc

### Testing
- @nestjs/testing, Jest is the default
- Test.createTestingModule()
    - useMocker(): helps mocking dependencies
    - overrideModule(RealModule).useModule(MockedModule)
- resolve(): get request or transient scoped dependency
- mock global dependency: use useExisting not useClass + provide the class after that
- use ContextIdFactory
- e2e: createNestApplication() to create a full Nest runtime

### Configuration
- a dedicated package to deal with .env files, uses dotenv
- ConfigModule.forRoot()
    - use the load config property to use env factory functions in which we can make some validations too
    - there is a cache config option
    - validationSchema config: use with Joi for validation
        - allowUnknown and abortEarly configs
    - OR use a custom validate function
    - create a configuration namespace with registerAs('namespaceName', ()=>{}) then you can load it by the name or inject it
        - we can use asProvider() of a namespace config to use that as a config provider for another module's forRootAsync
- ConfigModule.forFeature() for feature modules
- before bootstrapping we can use --env-file (Node v20)
- inject ConfigService to get the values
    - use the infer config property to infer the types
    - get nested values by using dot notation
- await ConfigModule.envVariablesLoaded: use this if a module config depends on loading the .env file
- ConditionalModule.registerWhen(FooModule, 'USE_FOO'): this is part of a module's import and that module is only loaded for FooModule whtn the USE_FOO env var is true; the condition can be a function too
    - the ConfigModule must be used for this to work
- expanded variable are supported: SUPPORT_EMAIL=support@${APP_URL} (uses dotenv-epanded)
    - set expandVariables to true in the ConfigModule
- usage in main.ts: app.get(ConfigService)

### Validation
- pipes: Validation, ParseInt, ParseBool, ParseArray, ParseUUID
- class-validator + class-transformer
- use classes not interfaces as DTOs
- steps to apply:
    - app.useGlobalPipes(new ValidationPipe()) to register globally
    - use the DTOs (or primitives) as the types of e.g. @Body
    - in the DTOs use decorators for the properties e.g. @IsEmail() from class-validator
- some config options:
    - whitelist: remove properties not having a decorator in the DTO
    - transform: transform the incoming object to the DTO
- helper types:
    - create a type for update from the create DTO: PartialType makes the properties optional
    - PickType: pick a few properties
    - OmitType: remove a few properties
    - IntersectionType: combine multiple types
- when the DTO is an array, use ParseArrayPipe: @Body(new ParseArrayPipe({ items: CreateUserDto })) createUserDtos: CreateUserDto[]

### Caching
- a dedicated package + cache-manager
- by default stuff is stored in memory but it can be changed
- CacheModule.register()
    - default TTL is 0 (never expires)
- @Inject(CACHE_MANAGER) private cacheManager: Cache
- async set, get, del and clear methods
- @UseInterceptors(CacheInterceptor) to cache GET responses which don't use the native @Res
- if it's globally enabled, CacheKey is based on the route path
    - we can overwrite @CacheTTL and @CacheKey
- not tracking by URL: extend CacheInterceptor and override trackBy

### Serialization
- ClassSerializerInterceptor + class-transformer
- @UseInterceptors(ClassSerializerInterceptor)
- e.g. @Exclude password in the response type class
    - the return type must be an instance of our class
    - OR use @UseInterceptors(ClassSerializerInterceptor) which creates an instance from our plain object to our return type class
- @Expose: alias a property name or create a getter
- @Transform: e.g. return the name of a child object and not the object itself
- @SerializeOptions on the route to config the interceptor

### Versioning
- use different versions of the controllers or individual routes
- types: URI (default), header, media type (Accept header like Accept: application/json;v=2), custom (create an extractor function, the requester might send an array of e.g. numbers)
- app.enableVersioning({type: VersioningType.URI})
- setting the version:
    - @Controller({version: '1'})
    - @Version('1') on a route
    - VERSION_NEUTRAL is version doesn't matter
- you can set default level(s) through the app variable
- middlewares can be versioned too through MiddlewareConsumer.forRoutes() in a module

### Task scheduling
- like cron
- ScheduleModule
- use @Cron() on the method
    - the method will be automatically wrapped in a try-catch
- CronExpression.EVERY_30_SECONDS enum for common stuff
- we can create a cron job dynamically by using the Dynamic API
- @Interval(10000) on a method (automatically wrapped in try-catch)
- @Timeout(5000) (also in try-catch)
- add a name prop to those above to handle them programatically
    - inject SchedulerRegistry and use the name to get them
    - SchedulerRegistry addCronJob(), interval, timeout

### Queues
- supports BullMQ (Redis under the hood)
- producer: @InjectQueue('audio') private audioQueue: Queue
- consumer: @Processor('audio') on a class that extends WorkerHost
    - @OnWorkerEvent('active') for events in a queue

### Logging
- has a Logger class
- config is set here: await NestFactory.create(AppModule, {} // config)
    - disable, colors, prefix, json, timestamp etc.
- levels: log, fatal, error, warn, debug, verbose
- usually we create an instance of the Logger class in each service
- custom logger: implement the LoggerService 
    - OR extend the original and override the methods
- we can also use DI with the logger

### Cookies
- cookie-parser is used (or @fastify/cookie)
- app.use(cookieParser())
    - we can config secret and options (for cookie.parse)
- result: req.cookies and req.signedCookies
- send cookie: response.cookie()

### Events
- EventEmitterModule from a dedicated package (uses EventEmitter2)
- inject EventEmitter2 to emit
    - use waitUntilReady to make the available to not trigger it before or during the onApplicationBootstrap lifecycle hook
- listen to an event with @OnEvent('order.created')

### Compression
- usually a reverse proxy (e.g. Nginx) is used for this
- compression or @fastify/compress
- app.use(compression())

### File upload
- uses the package multer, POST request must be multipart/form-data, doesn't work with Fastify
- use @UseInterceptors(FileInterceptor('file')) on controller methond and get the file using @UploadedFile() file: Express.Multer.File as an argument
- ParseFilePipe for validating the file
- use @UseInterceptors(FilesInterceptor('files')) and @UploadedFiles() in case of an array of files (single field name)
    - if there are more than 1 field names, use @UseInterceptors(FileFieldsInterceptor([{ name: 'avatar', maxCount: 1 },{ name: 'background', maxCount: 1 },]))
    - if there us no field name: @UseInterceptors(AnyFilesInterceptor())
- allow multipart/form-data but disable files beind uploaded: @UseInterceptors(NoFilesInterceptor())

### Streaming files
- use StreamableFile and pass a Buffer or a Stream

### HTTP module
- HttpModule and HttpService, uses axios

### Session
- uses express-session, apply its middleware globally
    - @fastify/secure-session for fastify
- read the value e.g. request.session.visits
    - or @Session() session: Record<string, any> on an argument

### Model View Controller
- we have to install a tempalte engine e.g. hbs
    - for fastify: @fastify/static @fastify/view
- we have to set
    - useStaticAssets
    - setBaseViewsDir: where our views are
    - setViewEngine
- use @Render('index') no a controller method
    - or use res.render() to dynamically return view on the same route

### Performance (Fastify)
- @nestjs/platform-fastify
- use FastifyAdapter when calling NestFactory.create()

### Server-Sent Events
- use @Sse('sse') on a controller method
- must return an Observable stream
- on the FE:
    - new EventSource('/sse')
    - eventSource.onmessage(cb)

### Database
- tight integration with TypeORM, Sequelize and Mongoose
- Repository pattern (TypeORM)
    - we have @Entity() classes like User where the columns are defined
    - create a service in which we inject the repository: @InjectRepository(User)
    - use subscribers to listen to entity events like beforeInsert etc
    - while testing we can use getRepositoryToken() to mock the repositories

### Mongo
- use TypeORM or mongoose + NestJs package (will talk about this)
- use @Schema() to create a schema e.g. Cat
- we can use the native Mongoose Connection: @InjectConnection() private connection: Connection
- hooks/middleware: pre and post events like pre-save
- while testing use getModelToken() to mock the models
- @Virtual property in the schema: like a computed prop

### Authentication
- @nestjs/jwt
- JwtModule.register({secret: ''}) etc
- automatically checks the token expiration
- @nestjs/passport
- if we set our auth guard to be global, we have to create a custom decorator so we can check its value and skip auth based in the value e.g. @Public

### Authorization
- RBAC: role-based access control
    - create a Role enum (user, admin)
    - create a @Roles decorator
    - then in the auth guard check if the user has the required roles
- claims based: describes what the user can do and not what the user is (that is RBAC)
- CASL (isomorphic authorization): create an ability about what a user can or cannot do on a resource
- automatic 403 response on failure

### Encryption and hashing
- encryption: use the built in Node module
- hashing: e.g. bcrypt or argon2
    - bcrypt.hash(pw, salrOrRounds)
    - bcrypt.genSalt()
    - bcrypt.compare(pw, hash)

### Helmet
- sets some HTTP headers
- must be registered before any routes are registered or app.use() is used

### CORS
- uses the cors package from Express
- app.enableCors(): pass a config object or a callback

### CSRF
- csrf-csrf package: requires session middleware or cookie-parser

### Rate limiting
- @nestjs/throttler
- ThrottlerModule: set name, ttl, limit
    - ttl 1000 + limit 10 = in 1 sec 10 request is allwed
- @SkipThrottle() to skip
- @Throttle({ default: { limit: 3, ttl: 60000 } }) to change the global
- by default it uses an in memory storage but it can be changed

### Router module
- use RouterModule and register the prefix for your controllers in a module and its child modules

### Serve static
- @nestjs/serve-static: use ServeStaticModule to configure the path to the static files

### Passport
- @nestjs/passport, @nestjs/jwt
- local strategy
    - need a custom class which extends PassportStrategy: has a validate() method that returns the user info (will be added to req) if the name and password is ok otherwise throws an error
        - or returns an array where the first element is the user, the second element is an authInfo object
    - call super() in the constructor if we need to config the strategy
    - @UseGuards(AuthGuard('local')) on the login method invokes the Passport strategy
    - use req.logout()
- jwt strategy
    - JwtModule and register e.g. secret, expiresIn etc
    - jwtService.sign(payload)
    - @UseGuards(AuthGuard('jwt'))