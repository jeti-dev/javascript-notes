---
title: Angular
layout: default
---

# Angular

## 🟢 **Basic Features**

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

## 🟡 **Intermediate Features**

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

## 🔴 **Advanced Features**

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

## Signals

| API              | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `signal()`       | Creates a reactive state signal.                                            |
| `computed()`     | Creates a derived value that auto-updates based on dependencies.           |
| `effect()`       | Triggers side effects when dependent signals change.                       |
| `input()`        | Binds a signal to an `@Input()` for reactive component inputs.             |
| `output()`       | Binds a signal to an `@Output()` to emit values reactively.                |
| `model()`        | Two-way binding: combines `input()` and `output()` signals.                |
| `untracked()`    | Accesses a signal value without registering it as a dependency.            |
| `set()`          | Sets a new value for a signal.                                              |
| `update()`       | Updates a signal’s value based on its current state.                       |
| `asReadonly()`   | Returns a readonly version of a signal to prevent external modifications.  |
| `isSignal()`     | Checks whether a given value is a signal.                                  |


## `signal()`

| Feature / Concept       | Description                                                                                   | Example                                                                                   |
|-------------------------|-----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| `signal()`              | Creates a reactive signal (a special kind of state variable).                                 | `const counter = signal(0);`                                                              |
| `computed()`            | Creates a derived signal that updates automatically based on other signals.                  | `const double = computed(() => counter() * 2);`                                           |
| `effect()`              | Runs a side-effect when one or more signals change.                                           | `effect(() => console.log(counter()));`                                                   |
| Reading a signal        | Invoke the signal like a function to read its current value.                                 | `counter()`                                                                               |
| Writing a signal        | Use `.set()`, `.update()` or `.mutate()` to change the signal's value.                       | `counter.set(10);` / `counter.update(v => v + 1);`                                        |
| `set()`                 | Replaces the signal's value.                                                                 | `counter.set(5);`                                                                         |
| `update()`              | Updates the signal based on its current value.                                               | `counter.update(c => c + 1);`                                                             |
| `mutate()`              | Mutates the value in-place (for arrays/objects) and notifies subscribers.                    | `list.mutate(arr => arr.push('item'));`                                                  |
| `Signal` vs `BehaviorSubject` | Signals are synchronous, fine-grained, and optimized for UI.                      | Signals avoid manual subscriptions/unsubscriptions compared to RxJS subjects.            |
| Signal Lifecycle        | Tied to component lifecycle, cleaned up automatically.                                        | No need to manually unsubscribe as with `Observable`.                                     |
| Integration with DOM    | Used directly in templates with `signal()` properties or `computed()` values.                | `<div>{{ counter() }}</div>`                                                              |
| Interoperability        | Works alongside existing Angular features (Inputs, Outputs, etc.)                            | Can use signals with `@Input()` and `@Output()` properties.                              |
| Signal with Input       | Convert `@Input()` to signal via `inputSignal()` (experimental)                              | `const myInput = inputSignal(this, 'myInput');`                                           |
| Use in Templates        | Call signal values directly in interpolation or bindings.                                    | `<span>{{ mySignal() }}</span>`                                                           |

##  `effect()` 

| Feature               | Description                                                                 | Example                                                                                   |
|-----------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Purpose               | Registers side effects that run when dependent signals change               | `effect(() => console.log(mySignal()));`                                                  |
| Automatic Dependency  | Tracks signal reads inside and re-runs effect when they change              |                                                                                           |
| Cleanup Support       | Supports cleanup functions using return or `onCleanup()`                    | `effect(() => { const sub = obs$.subscribe(); return () => sub.unsubscribe(); })`         |
| Lifecycle Aware       | Automatically destroyed when the component is destroyed                     | No need to manually unsubscribe                                                           |
| `onCleanup()`         | Registers a cleanup function inside an effect                               | `effect(() => { onCleanup(() => console.log('cleaned up')); })`                          |
| Multiple Dependencies | Can track multiple signals used within the effect body                      | `effect(() => console.log(signalA(), signalB()));`                                       |
| Avoid Complex Logic   | Should not be used for complex control flows or async logic (prefer RxJS)   | Use for simple DOM updates, logging, or calling services                                  |
| Return Value          | The return value is treated as a cleanup function                           | `return () => console.log('Effect cleanup');`                                             |
| Execution Timing      | Executes immediately once, then again whenever any dependency changes       |                                                                                           |
