---
title: Angular
layout: default
---


# 🅰️ Angular Concepts Cheat Sheets

## 📘 Beginner Angular Concepts

| Concept               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| Components            | Building blocks of UI, defined with a TypeScript class, template, and CSS. |
| Modules               | Organize app features into cohesive blocks (e.g., AppModule).               |
| Templates             | HTML + Angular syntax for binding data and events.                         |
| Data Binding          | Sync data between component and template (e.g., `{{ }}`, `[property]`, `(event)`). |
| Directives            | Special syntax in templates (e.g., `*ngIf`, `*ngFor`).                      |
| Services              | Reusable business logic and data handling, injected via DI.                |
| Dependency Injection  | Angular's system for providing services and dependencies.                   |
| Routing               | Define navigation paths and components via `RouterModule`.                  |
| CLI                   | Angular CLI to scaffold, build, test, and serve Angular apps.               |
| Lifecycle Hooks       | Component lifecycle methods (e.g., `ngOnInit`, `ngOnDestroy`).             |

---

## 📙 Intermediate Angular Concepts

| Concept                 | Description                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| Reactive Forms          | Form handling using observable streams and control objects.                |
| Template-Driven Forms   | Simpler form handling directly in the template.                            |
| Observables (RxJS)      | Angular uses RxJS to handle async streams and events.                      |
| HttpClient              | Service for making HTTP requests and managing responses.                   |
| Input/Output            | Pass data between parent and child components via `@Input` and `@Output`.  |
| Pipes                   | Transform output in the template (e.g., `date`, `uppercase`, custom pipes).|
| Guards                  | Control route access using `CanActivate`, `CanDeactivate`, etc.            |
| Lazy Loading Modules    | Load feature modules only when needed to optimize performance.             |
| Environment Configs     | Manage app settings via environment files.                                 |
| Angular Material        | UI component library for fast and responsive UI.                           |

---

## 📕 Advanced Angular Concepts

| Concept                      | Description                                                               |
|------------------------------|---------------------------------------------------------------------------|
| Standalone Components        | Components that don't need to be declared in a module.                   |
| Signals                      | Fine-grained reactive primitives to manage reactivity in Angular.        |
| Change Detection Strategy    | Optimize performance with `OnPush` strategy for components.             |
| ViewChild/ViewChildren       | Access DOM elements or components inside templates.                      |
| Content Projection           | Insert external content into components using `<ng-content>`.           |
| Dynamic Components           | Load components at runtime using `ViewContainerRef`.                    |
| State Management (NgRx)      | Manage app state using Redux-like store and RxJS.                        |
| Custom Directives            | Create reusable template logic via directive classes.                    |
| Custom Pipes                 | Transform data for display in a reusable way.                            |
| SSR / Angular Universal      | Server-side rendering for better SEO and faster initial load.            |
