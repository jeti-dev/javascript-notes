---
title: Javascript Frameworks
layout: default
---

# Javascript Frameworks

## Angular Cheatsheet

### 🟢 **Basic Features**

| Feature | Description | Example |
|---------|-------------|---------|
| **Component** | Building block of UI; contains HTML, CSS, and TypeScript. | `@Component({ selector: 'app', template: '<h1>Hello</h1>' })` |
| **Module** | Groups related components, pipes, and services. | `@NgModule({ declarations: [AppComponent] })` |
| **Template** | HTML structure of a component. | `<h1>{{ title }}</h1>` |
| **Interpolation** | Binds component properties to the template. | `<h1>{{ title }}</h1>` |
| **Property Binding** | Binds data from the component to the template. | `<img [src]="imageUrl">` |
| **Event Binding** | Binds DOM events to component methods. | `<button (click)="onClick()">Click Me</button>` |
| **Two-Way Binding** | Syncs data between component and template. | `<input [(ngModel)]="name">` |
| **Directive** | Modifies DOM elements. | `*ngIf="condition"` |
| **Service** | Reusable business logic, injected via DI. | `@Injectable()` |
| **Dependency Injection** | Provides instances of services where needed. | `constructor(private myService: MyService)` |
| **Lifecycle Hooks** | Executes code at specific component lifecycle stages. | `ngOnInit(), ngOnDestroy()` |

---

### 🟡 **Intermediate Features**

| Feature | Description | Example |
|---------|-------------|---------|
| **Router** | Manages navigation between views. | `RouterModule.forRoot(routes)` |
| **Lazy Loading** | Loads modules on demand. | `loadChildren: () => import('./module').then(m => m.Module)` |
| **Guards** | Controls access to routes. | `canActivate: [AuthGuard]` |
| **Resolvers** | Pre-loads data before route activation. | `resolve: { data: MyResolver }` |
| **Forms** | Template-driven or reactive forms for user input. | `FormBuilder.group({...})` |
| **HTTP Client** | Handles HTTP requests and responses. | `this.http.get('/api/data')` |
| **Pipes** | Transforms data in templates. | `{{ date \| date:'short' }}` |
| **Custom Pipes** | Create custom data transformations. | `@Pipe({ name: 'myPipe' })` |
| **Template Reference** | Direct access to a DOM element or component. | `#myInput` |
| **Content Projection** | Pass dynamic content to a component. | `<ng-content></ng-content>` |
| **Structural Directive** | Changes the DOM structure. | `*ngFor, *ngIf` |
| **Attribute Directive** | Changes the appearance of an element. | `[ngClass], [ngStyle]` |

---

### 🔴 **Advanced Features**

| Feature | Description | Example |
|---------|-------------|---------|
| **Change Detection** | Detects and responds to state changes. | `ChangeDetectionStrategy.OnPush` |
| **Zone.js** | Handles async operations and updates the view. | `import 'zone.js'` |
| **State Management** | Manages app state (e.g., NgRx, Akita). | `StoreModule.forRoot(reducers)` |
| **Dynamic Components** | Creates components at runtime. | `ViewContainerRef.createComponent()` |
| **Custom Decorators** | Creates reusable metadata annotations. | `function MyDecorator() { ... }` |
| **Web Workers** | Runs code in background threads. | `new Worker('./my-worker.js')` |
| **Service Workers** | Caches and manages network requests. | `ng add @angular/pwa` |
| **Directives with Inputs and Outputs** | Passes data into and out of a directive. | `@Input(), @Output()` |
| **DI Hierarchy** | Different levels of dependency providers. | `providedIn: 'root'` |
| **Feature Modules** | Organizes code into separate modules. | `NgModule({ imports: [...] })` |
| **SSR (Server-Side Rendering)** | Renders the app on the server for better SEO and performance. | `ng add @nguniversal/express-engine` |
| **Testing** | Unit and integration testing with Jasmine and Karma. | `TestBed.createComponent()` |
| **Internationalization (i18n)** | Supports multiple languages. | `ng add @angular/localize` |
