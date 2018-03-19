# Angular DI

The `@Injectable()` decorator identifies a service class that might require injected dependencies.

Angular module providers (@NgModule.providers) are registered with the application's root injector. unless the module is lazy loaded. Once created, a service instance lives for the life of the app and Angular injects this one service instance in every class that needs it.

A component's providers (@Component.providers) are registered with each component instance's own injector. Angular can only inject the corresponding services in that component instance or one of its descendant component instances. Each new instance of the component gets its own instance of the service and, when the component instance is destroyed, so is that service instance.