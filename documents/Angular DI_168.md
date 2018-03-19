# Angular DI

The `@Injectable()` decorator identifies a service class that might require injected dependencies.

Angular module providers (@NgModule.providers) are registered with the application's root injector. unless the module is lazy loaded. Once created, a service instance lives for the life of the app and Angular injects this one service instance in every class that needs it.

A component's providers (@Component.providers) are registered with each component instance's own injector. Angular can only inject the corresponding services in that component instance or one of its descendant component instances. Each new instance of the component gets its own instance of the service when the component instance is destroyed, so is that service instance. roviding the service at the component level ensures that every instance of the component gets its own, private instance of the service.

Services are singletons within the scope of an injector. There is at most one instance of a service in a given injector.  

When Angular destroys one of these component instance, it also destroys the component's injector and that injector's service instances. A component's injector is a child of its parent component's injector, and a descendent of its parent's parent's injector, and so on all the way back to the application's root injector

The @Optional decorator tells Angular to continue when it can't find the dependency. Angular sets the injection parameter to null instead.

The @Host decorator stops the upward search at the host component. he host component is typically the component requesting the dependency. But when this component is projected into a parent component, that parent component becomes the host. The next example covers this second case.

## Aliased class providers

```js
[ NewLogger,
  // Not aliased! Creates two instances of `NewLogger`, one for NewLogger and one for OldLogger
  { provide: OldLogger, useClass: NewLogger}]
```

```js
// An object in the shape of the logger service
[ NewLogger,
  // Alias OldLogger w/ reference to NewLogger, only one NewLogger instance
  { provide: OldLogger, useExisting: NewLogger}]
```

TypeScript interfaces aren't valid tokens, An interface is a TypeScript design-time artifact. JavaScript doesn't have interfaces. The TypeScript interface disappears from the generated JavaScript. There is no interface type information left for Angular to find at runtime. One solution is using InjectionToken.

## Working with injectors directly

```js 
constructor(private injector: Injector) { }

ngOnInit() {
  this.car = this.injector.get(Car);
  this.heroService = this.injector.get(HeroService);
  this.hero = this.heroService.getHeroes()[0];
}
```

## Hierarchical Dependency Injectors

When a component requests a dependency, Angular tries to satisfy that dependency with a provider registered in that component's own injector. If the component's injector lacks the provider, it passes the request up to its parent component's injector. If that injector can't satisfy the request, it passes it along to its parent injector. The requests keep bubbling up until Angular finds an injector that can handle the request or runs out of ancestor injectors. If it runs out of ancestors, Angular throws an error.

