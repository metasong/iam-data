# Angular DI

The `@Injectable()` decorator identifies a service class that might require injected dependencies.

Angular module providers (@NgModule.providers) are registered with the application's root injector. unless the module is lazy loaded. Once created, a service instance lives for the life of the app and Angular injects this one service instance in every class that needs it.

A component's providers (@Component.providers) are registered with each component instance's own injector. Angular can only inject the corresponding services in that component instance or one of its descendant component instances. Each new instance of the component gets its own instance of the service and, when the component instance is destroyed, so is that service instance.

Services are singletons within the scope of an injector. There is at most one instance of a service in a given injector.  

When Angular destroys one of these component instance, it also destroys the component's injector and that injector's service instances. A component's injector is a child of its parent component's injector, and a descendent of its parent's parent's injector, and so on all the way back to the application's root injector

## Aliased class providers

```js
[ NewLogger,
  // Not aliased! Creates two instances of `NewLogger`
  { provide: OldLogger, useClass: NewLogger}]
```

```js
// An object in the shape of the logger service
export function SilentLoggerFn() {}

const silentLogger = {
  logs: ['Silent logger says "Shhhhh!". Provided via "useValue"'],
  log: SilentLoggerFn
};
```