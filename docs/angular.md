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

## Angular docs notes
- signal()
    - const val = signal('default value')
    - val()
    - val.set('new value')
    - val.update(prevValue => prevValue + 1)
    - works with onPush
    - can have an equality function to decide if we need to rerender
- computed()
    - read only, changes when any of the used signals change
    - const comp = computed(()=> val().toUpperCase())
- inject(MyProvider)
    - inject(ElementRef)
- effect((onCleanup) => {})
    - runs at least once
    - async
    - tracks dependencies as computed() does
    - can be used only within the injection context: within a component, directive or service constructor OR use the Injector
    - cleaned up automatically
    - can be cleaned up by calling .destroy()
    - untracked(mySignal): reads the signal value without creating a dependency on it OR if a function is given as a paramter, it won't track that functions' signals as dependency
    - onCleanup: runs before the next effect or when the effect is destroyed
- input.required<number>(defValue, {transform: trimString, alias: 'attrName'})
    - @Input is still supported
    - @Component() may have an inputs config which shows what inputs were inhertied from a base component
- model(defValue)
    - const myModel = model(): a myModelChanged output is automatically created
    - can be customized as the input()
- output()
    - const myOutput = output<void>({alias: 'myAlias'}) -> myOutput.emit()
    - when creating a component dynamically, you can subscribe() to the events manually
        - these subscriptions will be cleaned up automaically
        - you can still .unsubscribe()
    - @Output is still supported
    - @Component might have an outputs config which shows what outputs were inherited from a base component
- viewChild(CompName)
    - viewChild<ElementRef<HTMLButtonElement>>('save') where save is #save in the template
- viewChildren(CompName)
- contentChild(CompName): from inside <ng-content>
    - {descendants: true}: allow to search for not direct children
- contentChildren(CompName): from inside <ng-content>
- @ViewChild, @ContentChild etc are still supported
- directive template reference: the directive must use the exportAs config
- programatically rendering components:
    - <ng-container *ngComponentOutlet="getComp()" /> where getComp() returns a component class name: creates the comp in the container
    - OR: inject(ViewContainerRef) then viewContainer.createComponent(MyComp): creates the component as the next sibling
    - both can be lazy loaded

- @if, @else-if, @else, @for, @empty, @switch, @let
- @defer{ <my-comp></my-comp> }: create a separate js file for the comp and load only on some condition
    - must be standalone, must not be referenced in the same file e.g. ViewChild
    - by default it loads then the browser becomes idle
    - @placeholder: what is shown before @defer loads the content, can set minimum time to show in case @defer is too fast
    - @loading: while loading, can set minimum time to show and after that describes how much time after the loading started we should show the @loading block
    - @error: when the @defer fails
    - condition - on: idle, viewport, interaction, hover, immediate, timer
    - condition - when: custom callback
    - prefetch: we can define if we want to preload the js on some cindition before we show the content
    - for SSG and SSR @placeholder is rendered by default but it can be changed to @defer
- pipe: by default they are pure -> they are triggered only when their input changes
- <ng-template></ng-template>: a reuseable template
    - they are not rendered by default, so you needto reference them:
        - <ng-template #myFragment></ng-template>
        - @ViewChild(TemplateRef) myFragment: TemplateRef
        - @ViewChild('fragmentOne', {read: TemplateRef}) fragmentOne: TemplateRef
        - inject(TemplateRef): only for directives if they are on the template
    - render:
        - <ng-container *ngTemplateOutlet="myFragment"></ng-container>
        - OR: this.viewContainer.createEmbeddedView(this.fragment);
    - adding some parameters: 
        - <ng-template let-pizzaTopping="topping"></ng-template>
        - then: [ngTemplateOutletContext]="{topping: 'onion'}"
        - OR: this.viewContainer.createEmbeddedView(this.myFragment, {topping: 'onion'})

- ProviderToken: locate by component and directive providers
- component selectors: my-comp, [type="reset"], .my-class, :not()
- style scoping: emulated, shadow dom, none
- content projection: <ng-content select="card-title">Default content to show</ng-content>
    - it becomes: <card-title>...</card-title> in the parent component's tempalte
        - OR: <h3 ngProjectAs="card-title">My title</h3>
    - if you have an ng-content without a select attribute, all other injected content are rendered in that
- you can bind stuff on the host element in the @Component's config using the host: {} config
    - OR: @Hostbinding, @Hostlistener on the properties of the component
- OnPush is triggered when:
    - an input() changes which is binded in the template
    - event listener in the component
    - the component is ChangeDetextorRef.markForCheck or something that wraps it like AsyncPipe
- create custom elements: use the package @angular/elements
- NgOptimizedImage: ensures that the LCP is prioritized e.g. sets fetchpriority on the <img>
    - fill: when you don't know the exact size of the image
    - a lot of other stuff https://angular.dev/guide/image-optimization
- @Injectable:
    - definition at:
        - root level: preferred, can be tree-shaken
        - component level
        - app level with ApplicationConfig
    - [{ provide: Logger, useClass: Logger }]
        - or: useExisting, useFactory, useValue
    - InjectionToken for non-class deps: const APP_CONFIG = new InjectionToken<AppConfig>('app.config description')
    - lightweight injection token: Some stuff in a library can be added to the parent code even if that stuff is not used. e.g. @ContentChild (a card comp has an optional header comp)
        - solution: Don't use the header comp directly as an injection token, but use an abstract class that is implemented by the real header comp.

### Angular Component Lifecycle Hooks (Complete)

| Hook Name                 | Timing of Execution                                                  | Purpose                                                                 |
|---------------------------|----------------------------------------------------------------------|-------------------------------------------------------------------------|
| `ngOnChanges(changes)`    | Before `ngOnInit()`, when any bound `@Input()` property changes      | Respond to changes in input properties.                                |
| `ngOnInit()`              | Once after the first `ngOnChanges()`                                 | Initialize the component; ideal for fetching data or setting up state. |
| `ngDoCheck()`             | During every change detection cycle                                  | Custom change detection beyond default mechanisms.                     |
| `ngAfterContentInit()`    | Once after content (e.g. `<ng-content>`) is projected into the view  | Act after projected content is initialized.                            |
| `ngAfterContentChecked()` | After every check of projected content                               | Respond after Angular checks the content projected into the component. |
| `ngAfterViewInit()`       | Once after component's view (and its children) are initialized       | Perform DOM-dependent logic or interact with child views.              |
| `ngAfterViewChecked()`    | After every check of component’s and children’s views                | Respond after Angular checks the component view and child views.       |
| `ngOnDestroy()`           | Just before Angular destroys the component                           | Cleanup work like unsubscribing from observables or detaching handlers.|
| `afterNextRender()`       | Runs once the next time that all components have been rendered to the DOM. | Cleanup work like unsubscribing from observables or detaching handlers.|
| `afterRender()`           | Runs every time all components have been rendered to the DOM.        | Cleanup work like unsubscribing from observables or detaching handlers.|

### Hierarchical Injectors in Angular

| Level                | Description                                                                 | Typical Use Cases                                         | Notes                                                                 |
|----------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------------------------------------------|
| **Platform Injector** | Created by Angular itself, shared across all Angular applications           | Rarely used directly by developers                        | Provides services like DOM-related utilities                          |
| **Root Injector**     | Created by `bootstrapApplication()` or `AppModule`                         | App-wide singleton services                               | Services provided in `@Injectable({ providedIn: 'root' })` are here   |
| **Module Injector**   | Injector created for lazy-loaded feature modules                           | Services needed only in a lazy-loaded module              | Services are destroyed when module is unloaded                        |
| **Component Injector**| Unique injector per component instance                                      | Provide component-specific or overridden service instances| Created via `providers: [...]` in component metadata                  |
| **Directive Injector**| Exists for directives declared on a host element                           | Provide directive-specific services                       | Useful for customizing behavior via DI in shared directives           |
| **Element Injector**  | Created for each DOM element with a directive/component                    | Platform-specific internal mechanics                      | Rarely interacted with directly                                       |


- Angular injectors form a tree structure (hierarchy), and child injectors can **override** parent-provided services.
- Angular resolves dependencies by **walking up the injector tree** until it finds the required provider.
- Providing a service at a **lower level** creates a **new instance** and **overrides** higher-level singleton instances.

### Comparison: `providers` vs `viewProviders` in Angular

| Feature                  | `providers`                                              | `viewProviders`                                          |
|--------------------------|----------------------------------------------------------|----------------------------------------------------------|
| **Scope**                | Available to the component, its content children, and view children | Available only to the component and its view children     |
| **Affects Content Projection (`<ng-content>`)** | ✅ Yes                                                   | ❌ No                                                     |
| **Used In**              | `@Component`, `@Directive`, NgModules                    | Only in `@Component`                                     |
| **Override Capability**  | Can override services from parent injectors              | Can also override, but only within the view hierarchy     |
| **Typical Use Case**     | When projected content also needs access to the service  | When service should be isolated from projected content    |
| **Example Use Case**     | Shared form services between component and projected content | Internal service for template logic only                 |
| **Syntax**               | `providers: [MyService]`                                 | `viewProviders: [MyService]`                             |
| **Visibility**           | Component + Content + View                              | Component + View only (not visible to projected content)  |

- Use `viewProviders` for internal-only services (e.g., UI state, internal logic).
- Use `providers` when the service must be available to content projected via `<ng-content>`.
