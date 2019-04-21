# Angular

### 1) trackBy
When using ngFor to loop over an array in templates, use it with a trackBy function which will return an unique identifier for each item.

```javascript
// in the template
<li *ngFor="let item of items; trackBy: trackByFn">{{ item }}</li>

// in the component
trackByFn(index, item) {    
   return item.id; // unique id corresponding to the item
}
```


### 2) Subscribe in template
Avoid subscribing to observables from components and instead subscribe to the observables from the template.

```javascript
// template
<p>{{ textToDisplay$ | async }}</p>

// component
this.textToDisplay$ = iAmAnObservable
    .pipe(
       map(value => value.item)
     );
```

### 3) Clean up subscriptions
When subscribing to observables, always make sure you unsubscribe from them appropriately by using operators like take, takeUntil, etc.

```javascript
private _destroyed$ = new Subject();

public ngOnInit (): void {
    iAmAnObservable
    .pipe(
       map(value => value.item)
      // We want to listen to iAmAnObservable until the component is destroyed,
       takeUntil(this._destroyed$)
     )
    .subscribe(item => this.textToDisplay = item);
}

public ngOnDestroy (): void {
    this._destroyed$.next();
    this._destroyed$.complete();
}
```

[Best practices for a clean and performant Angular application](https://medium.freecodecamp.org/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f)

### 4) Single Responsibility Principle
It is very important not to create more than one component, service, directive… inside a single file. Every file should be responsible for a single functionality. By doing this, we are keeping our files clean, readable and maintainable.

### 5) Module Organization and Lazy Loading
Modules are very important in an Angular project, so let’s talk about that and lazy loading and shared modal features.

### 6) Multi Modules in Application
Even though an Angular application is going to work just fine if we create just one module, the recommendation is to split our application into multi-modules. There are a lot of advantages to this approach. The project structure is better organized, it is more maintainable, readable and reusable and we are able to use the lazy-loading feature.

### 7) Lazy Loading
If we have a multi-modular application, implementing a lazy loading feature is recommended. The great advantage of a lazy loading approach is that we can load our resources on demand and not all at once. This helps us in decreasing the startup time. Modules that we are loading in a lazy manner will be loaded as soon as a user navigates to their routes.

Let’s show how to set up lazy loading in our module:
```javascript
const appRoutes: Route[] = [
  { path: 'home', component: HomeComponent },
  { path: 'owner', loadChildren: "./owner/owner.module#OwnerModule" },
]
```
In this code example, the HomeComponent is loading eagerly but the OwnerModule and all the components registered in that module are loading in a lazy manner.

If you want to learn more about the lazy loading feature you may read [our blog post about lazy loading](https://code-maze.com/net-core-web-development-part10/).


[Angular Development Best Practices](https://code-maze.com/angular-best-practices/)
