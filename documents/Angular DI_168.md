# Angular DI

The `@Injectable()` decorator identifies a service class that might require injected dependencies.

Angular module providers (@NgModule.providers) are registered with the application's root injector. unless the module is lazy loaded. Angular can inject the corresponding services in any class it creates. 

A component's providers (@Component.providers) are registered with each component instance's own injector.Angular can only inject the corresponding services in that component instance or one of its descendant component instances.