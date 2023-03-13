# Angular

## General
- Front-end framework
- Used to be called **AngularJS** but in *version 2* it was rewritten completely from scratch
- Uses **TypeScript** by default
- Maintained by **Google**
- Higher learning curve than **React** and **Vue**
- Very mature and robust which makes it popular in enterprise
- Includes the **RxJS** library for *asynchronous programming*
- Uses double curly braces (`{{}}`) as its *interpolation binding syntax* (e.g. `<h1>{{ title }}</h1>`)

## Angular CLI
- Install with `npm install -g @angular/cli`
- Create a new project with `ng new <project-name>`
- Start a development server and open the browser to it with `ng serve --open`
- Create a component with `ng generate component <component-name>`
- Create a service with `ng generate service <service-name>`
- Create a module with `ng generate module <module-name>`
- Build for production with `ng build`

## Components
- **Components** are the fundamental building blocks of **Angular** applications
- They display data on the screen, listen for user input, and take action based on that input
- Every component has its own directory with 4 files in it:
  - `<name>.component.html` - the markup template for the component
  - `<name>.component.css` - the component stylesheet
  - `<name>.component.ts` - the component logic
  - `<name>.component.spec.ts` - the component test specification
- Application-wide styles are put in `src/styles.css`
- Components are implemented as **TypeScript** classes
- Every component is decorated with `@Component` which is a decorator function that specifies the **Angular** metadata
  for the component:
  - `selector` - the component's **CSS** element selector and the name of the **HTML** element that identifies this
      component within a parent component's template
  - `templateUrl` - the location of the component's template file
  - `styleUrls` - the location of the component's private **CSS** styles
- Properties need to be added to a component's `class` in order to be available for use in its template file
- `Pipes` are a good way to format strings, currency amounts, dates, and other display data  
  e.g. `<h2>{{hero.name | uppercase}} Details</h2>`

## Services
**Components** shouldn't fetch or save data directly and they certainly shouldn't knowingly present fake data. They should
focus on presenting data and delegate data access to a service.

**Services** are a great way to share information among classes that don't know each other.

Service classes are decorated with the `@Injectable()` decorator from `@angular/core`. This marks the class as one that
participates in the *dependency injection* system. It accepts a metadata object for the service, the same way the
`@Component` decorator does for the component classes.

We must make a service available to the *dependency injection* system before **Angular** can *inject* it into a
component by registering a **provider**. A **provider** is something that can create or deliver a service.

The **injector** is the object that chooses and injects the **provider** where the application requires it.

By default, `ng generate service` registers a **provider** with the **root injector** for a service by including
provider metadata; that's `providedIn: "root"` in the `@Injectable()` decorator.

When we provide the service at the root level, **Angular** creates a single, shared instance of it and injects it into
any class that asks for it. Registering the provider in the `@Injectable()` metadata also allows **Angular** to optimize
an application by removing the service if it isn't used.

To inject a service into a component, we `import` it and add it as a `private` parameter of the component's `constructor`.

While we could call a service method in the constructor, that's not the best practice. The constructor should be
reserved for minimal initialization such as wiring constructor parameters to properties. The constructor shouldn't do
*anything*. It certainly shouldn't call a function that makes **HTTP** request to a remote server as a *real* data
service would.

Instead, we should put that functionality inside the `ngOnInit` *lifecycle hook* and let **Angular** call `ngOnInit()`
at an appropriate time after constructing a component instance.

## AppModule
**Angular** needs to know how the pieces of our application fit together and what other files and libraries the
application requires. This information is called **metadata**.

Some of the critical metadata is in `@NgModule` decorators.

The most important `@NgModule` decorator annotates the top-level `AppModule` class.

`ng new` creates an `AppModule` class in `src/app/app.module.ts` when it creates the project.

To opt-in for modules like the `FormsModule` we need to import them and add them to the `imports` array in `@NgModule`.
For example:

```typescript
import { FormsModule } from "@angular/forms";
...
imports: [
  BrowserModule,
  FormsModule
],
```

Similarly, every component must by declared in exactly one `NgModule`:

```typescript
import { HeroesComponent } from "../components/heroes/heroes.component";
...
declarations: [
  AppComponent,
  HeroesComponent
],
```

## Directives
**Directives** are classes that add additional behavior to elements in **Angular** applications:

- `*ngFor` - present a list of items:
  ```html
  <div *ngFor="let item of items">{{item.name}}</div>
  ```
- `*ngIf` - conditionally include a template based on the value of an expression coerced to **Boolean**:
  ```html
  <div *ngIf="condition">Content to render when condition is true.</div>
  ```
- `[ngClass]` - add or remove multiple **CSS** classes simultaneously
- `[ngStyle]` - set multiple inline styles simultaneously, based on the state of the component
- `[(ngModel)]` - create a two-way binding between an input element an a component property (need to import the
  `FormsModule`):
  ```html
  <input id="name" [(ngModel)]="hero.name" placeholder="name">
  ```

## Types of data binding
**Angular** provides three categories of data binding according to the direction of data flow:

- From source to view (**Interpolation**, **Property**, **Attribute**, **Class**, **Style**):  
  `{{expression}}` or `[target]="expression"`
- From view to source (**Event**):  
  `(target)="statement"`
- In a two-way sequence of view to source to view:  
  `[(target)]="expression"`

## Sharing data between child and parent directives and components
A common pattern in **Angular** is sharing data between a parent component and one or more child components. We can
implement this pattern with the `@Input()` and `@Output()` decorators.

`@Input()` lets a parent component update data in the child component:

```typescript
/* item-detail.component.ts */
import { Component, Input} from "@angular/core";

export class ItemDetailComponent {
  @Input() item = ""; // decorate the property with @Input()
}
```

```html
<!-- item-detail.component.html -->
<p>Today's item: {{item}}</p>
```

```html
<!-- app.component.html -->
<app-item-detail [item]="currentItem"></app-item-detail>
```

```typescript
/* app.component.ts */
export class AppComponent {
  currentItem = "Television";
}
```

Conversely, `@Output()` lets the child send data to a parent component. The child component uses the `@Output()`
property to raise an event to notify the parent of the change. To raise an event, an `@Output()` must have the type of
`EventEmitter`, which is a class in `@angular/core` that we can use to emit custom events:

```typescript
/* item-output.component.ts */
import { Output, EventEmitter } from "@angular/core";

export class ItemOutputComponent {
  @Output() newItemEvent = new EventEmitter<string>();

  addNewItem(value: string) {
    this.newItemEvent.emit(value);
  }
}
```

```html
<!-- item-output.component.html -->
<label for="item-input">Add an item:</label>
<input type="text" id="item-input" #newItem>
<button type="button" (click)="addNewItem(newItem.value)">Add to parent's list</button>
```

```typescript
/* app.component.ts */
export class AppComponent {
  items = ['item1', 'item2', 'item3', 'item4'];

  addItem(newItem: string) {
    this.items.push(newItem);
  }
}
```

```html
<!-- app.component.html -->
<app-item-output (newItemEvent)="addItem($event)"></app-item-output>
```

## Observable data
`Observable` is one of the key classes in the **RxJS** library used for *asynchronous programming*.

The `of` function can be used to return an `Observable` that emits a single value, its parameter.

The `subscribe` method of an `Observable` accepts a callback function that gets passed the value emitted by the
`Observable` when it becomes available:

```typescript
getHeroes(): void {
  this.heroService.getHeroes().subscribe(heroes => this.heroes = heroes);
}
```

## HTTP services
`HttpClient` is **Angular**'s mechanism for communicating with a remote server over **HTTP**.

It is similar to [axios](https://axios-http.com/) or the **JavaScript** built-in `fetch` function.

`HttpClient` methods return **RxJS** `Observable` objects.

To make it available we must add the following to `app.module.ts`:

```typescript
import { HttpClientModule } from "@angular/common/http";
...
@NgModule({
  imports: [
    HttpClientModule,
  ],
})
```

Then to use it in a [service](#services):

```typescript
import { Injectable } from "@angular/core";
import { HttpClient, HttpHeaders } from "@angular/common/http";

import { Observable } from "rxjs";

import { Hero } from "./hero";

@Injectable({ providedIn: "root" })
export class HeroService {
  private heroesUrl = "api/heroes"; // URL to web API

  httpOptions = {
    headers: new HttpHeaders({ "Content-Type": "application/json" })
  };

  constructor(private http: HttpClient) {}

  getHeroes(): Observable<Hero[]> {
    return this.http.get<Hero[]>(this.heroesUrl);
  }

  addHero(hero: Hero): Observable<Hero> {
    return this.http.post<Hero>(this.heroesUrl, hero, this.httpOptions);
  }
}
```

### Routing
**Angular** includes its own *routing* module, similar to [Vue Router](https://router.vuejs.org/) and [react-router](https://reactrouter.com/en/main).

By convention, the module class name is `AppRoutingModule` and it belongs in the `app-routing.module.ts` in the
`src/app` directory.

```typescript
/* app-routing.module.ts */
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

We need to add `<router-outlet></router-outlet>` to `app.component.html` to tell the **router** where to display routed
views.

`a` tags can be used for navigation links, but with the `href` attribute replaced with the `routerLink` attribute. This
makes client-side routing work without page refreshes.
