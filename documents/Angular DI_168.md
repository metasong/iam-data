# Angular DI

Notes from [Angular DI](https://angular.io/guide/dependency-injection-pattern)

The `@Injectable()` decorator identifies a service class that might require injected dependencies.

Angular module providers (@NgModule.providers) are registered with the application's root injector. unless the module is lazy loaded. Once created, a service instance lives for the life of the app and Angular injects this one service instance in every class that needs it.

A component's providers (@Component.providers) are registered with each component instance's own injector. Angular can only inject the corresponding services in that component instance or one of its descendant component instances. Each new instance of the component gets its own instance of the service when the component instance is destroyed, so is that service instance. roviding the service at the component level ensures that every instance of the component gets its own, private instance of the service.

Services are singletons within the scope of an injector. There is at most one instance of a service in a given injector.  

When Angular destroys one of these component instance, it also destroys the component's injector and that injector's service instances. A component's injector is a child of its parent component's injector, and a descendent of its parent's parent's injector, and so on all the way back to the application's root injector

The @Optional decorator tells Angular to continue when it can't find the dependency. Angular sets the injection parameter to null instead.

The @Host decorator Specifies that an injector should retrieve a dependency from any injector until reaching the host element of the current component. the host component is typically the component requesting the dependency. But when this component is projected into a parent component, that parent component becomes the host. The next example covers this second case.

@SkipSelf is essential for two reasons:

* It tells the injector to start its search for a Parent dependency in a component above itself, which is what parent means.

* Angular throws a cyclic dependency error if you omit the @SkipSelf decorator. Cannot instantiate cyclic dependency! (BethComponent -> Parent -> BethComponent)
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

## Inject the component's DOM element

```js
  private el: HTMLElement;

  constructor(el: ElementRef) {
    this.el = el.nativeElement;
  }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || 'cyan');
  }
```

## forwardRef
forwardRef is used when the token which we need to refer to for the purposes of DI is declared, but not yet defined. It is also used when the token which we use when creating a query is not yet defined. could be used to solve this problem: You're in a bind when class 'A' refers to class 'B' and 'B' refers to 'A'. One of them has to be defined first.
```typescript
class Door {
  lock: Lock;

  // Door attempts to inject Lock, despite it not being defined yet.
  // forwardRef makes this possible.
  constructor(@Inject(forwardRef(() => Lock)) lock: Lock) { this.lock = lock; }
}

// Only at this point Lock is defined.
class Lock {}

const injector = ReflectiveInjector.resolveAndCreate([Door, Lock]);
const door = injector.get(Door);
expect(door instanceof Door).toBeTruthy();
expect(door.lock instanceof Lock).toBeTruthy();
```

## [Let’s see the code](https://medium.freecodecamp.org/angulars-backyard-the-resolving-of-component-dependencies-2015b40e5bd1)
 The full code can be found [here](https://github.com/angular/angular/blob/master/packages/core/src/view/provider.ts#L343).

```typescript
export function resolveDep(view: ViewData, elDef: NodeDef, allowPrivateServices: boolean, 
    depDef: DepDef, notFoundValue: any = Injector.THROW_IF_NOT_FOUND): any {
  
  const startView = view;
  const tokenKey = depDef.tokenKey;
  ...

  while (view) {
    if (elDef) {
      switch (tokenKey) {
        case RendererV1TokenKey: { ... }
        case Renderer2TokenKey: { ... }
        case ElementRefTokenKey: { ... }
        case ViewContainerRefTokenKey: { ... }
        case TemplateRefTokenKey: { ... }
        case ChangeDetectorRefTokenKey: { ... }
        case InjectorRefTokenKey: { ... }
        default:
          const providerDef =
              (allowPrivateServices ? elDef.element !.allProviders :
                                      elDef.element !.publicProviders) ![tokenKey];
          if (providerDef) {
            const providerData = asProviderData(view, providerDef.index);
            if (providerData.instance === NOT_CREATED) {
              providerData.instance = _createProviderInstance(view, providerDef);
            }
            return providerData.instance;
          }
      }
    }
    ...
    
    elDef = viewParentEl(view) !;
    view = view.parent !;
/*
The while loop will continue running until the dependency is found in the ancestor injector.
*/
  }

/*
If the source component or one of its ancestor components was loaded by the Router Outlet (the router component), the root injector is the Outlet Injector. This injector supplies some dependencies like the *Router* service. Otherwise, the root injector is the bootstrap component’s injector.
 */                                   
  const value = startView.root.injector.get(depDef.token, NOT_FOUND_CHECK_ONLY_ELEMENT_INJECTOR);

  if (value !== NOT_FOUND_CHECK_ONLY_ELEMENT_INJECTOR || ...) {
    return value;
  }
/*
tries to get the dependency from the application module injector or the root module. 
*/
  return startView.root.ngModule.injector.get(depDef.token, notFoundValue);
}

```

**Notes:**
* !. is non-null assertion operator - it just saying to type checker that you're sure that a is not null or undefined.  the operation x! produces a value of the type of x with null and undefined excluded. Similar to type assertions of the forms <T>x and x as T, the ! non-null assertion operator is simply removed in the emitted JavaScript code.
more info [here](https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript#non-null-assertion-operator)


