# angular injector tree
*summery*
https://blog.angularindepth.com/angular-dependency-injection-and-tree-shakeable-tokens-4588a8f70d5d

![=100%*](https://cdn-images-1.medium.com/max/2000/1*rjG7U4vLG_keRYoZnryxbA.png)

* MyService2 from EagerModule2 will be included into the root injector.
* `ComponentFactoryResolver` is also [added](https://github.com/angular/angular/blob/f258ec67bf20974362c33f59ec2be79fa9f8b9c8/packages/compiler/src/ng_module_compiler.ts#L36) to the root module injector by Angular. This resolver is responsible for _dynamic_ creation components since it stores factories of `entryComponents`.
*  Module Tokens are actually types of all merged NgModules.

> **Tip**: If you have angular application in dev mode and want to see all providers from root AppModule injector then just open devtools console and write:  
> `ng.probe(getAllAngularRootElements()[0]).injector.view.root.ngModule._providers`

the AppModule root injector has a parent NgZoneInjector , which is a child of PlatformInjector.

```ts
const platform = platformBrowserDynamic([ { 
  provide: SharedService, // shared static providers
  deps:[] 
}]);
platform.bootstrapModule(AppModule);
platform.bootstrapModule(AppModule2);
```
![=100%*](https://i.stack.imgur.com/PSvBz.png)

https://stackoverflow.com/questions/48594944/can-deps-also-be-used-with-useclass/48595518#48595518

## EntryComponent and RootData

Every time we create dynamic component angular creates root view with root data, that contains references to elInjector and ngModule injector.

These types of components are usually passed either in bootstrap or entryComponents array of NgModule. Angular router also creates component dynamically.

```ts
function createRootData(
    elInjector: Injector, ngModule: NgModuleRef<any>, rendererFactory: RendererFactory2,
    projectableNodes: any[][], rootSelectorOrNode: any): RootData {
  const sanitizer = ngModule.injector.get(Sanitizer);
  const errorHandler = ngModule.injector.get(ErrorHandler);
  const renderer = rendererFactory.createRenderer(null, null);
  return {
    ngModule,
    injector: elInjector, projectableNodes,
    selectorOrNode: rootSelectorOrNode, sanitizer, rendererFactory, renderer, errorHandler
  };
}
```


## MergeInjector


https://docs.google.com/document/d/1OEUIwc-s69l1o97K0wBd_-Lth5BBxir1KuCRWklTlI4/mobilebasic

Imagine that we have components cX, cY, cZ declared in module mX, mY and mZ respectively. Also assume that we want to load the components in the order of cX <- cY <- cZ (Where cX is the root, cY is the child and cZ is the grandchild, arrow implies parent relationship).  These components have associated injectors ciX <- ciY <- ciZ. Finally let’s assume that all components are being lazy loaded by the router and have corresponding guards gY and gZ which need to approve the loading of the component cY and cZ.

We will introduce a special merge Injector which will itself not have any values, but it will resolve values first in component injector and then in module injector. This merge injector will be private to the component.

```js
null <-- mX <--------- mY <--------- mZ

         ^             ^             ^

         |             |             |

         miX <- cX    miY <- cY     miZ <- cZ

         |             |             |

         v             v             v

null <- ciX <-------- ciY <-------- ciZ
```

* If cZ would ask for dependency Q it would be resolved in: ciZ, ciY, ciX, and then in mZ, mY, and mX.
* If cY would ask for dependency R it would be resolved in: ciY, ciX, and then in mY, and mX.


```ts
class Injector_ implements Injector {
  constructor(private view: ViewData, private elDef: NodeDef|null) {}
  get(token: any, notFoundValue: any = Injector.THROW_IF_NOT_FOUND): any {
    const allowPrivateServices =
        this.elDef ? (this.elDef.flags & NodeFlags.ComponentView) !== 0 : false;
    return Services.resolveDep(
        this.view, this.elDef, allowPrivateServices,
        {flags: DepFlags.None, token, tokenKey: tokenKey(token)}, notFoundValue);
  }
}

```

As we can see in the preceding code Merge injector is just combination of view and element definition.
![=100%*](https://cdn-images-1.medium.com/max/2000/1*WGyA-RolH_Z68SMks4FmJA.gif)

Merge injector:is just combination of view and element definition. This injector works like a bridge between element injector tree and module injector tree.
```ts
class Injector_ implements Injector {
  constructor(private view: ViewData, private elDef: NodeDef|null) {}
  get(token: any, notFoundValue: any = Injector.THROW_IF_NOT_FOUND): any {
    const allowPrivateServices =
        this.elDef ? (this.elDef.flags & NodeFlags.ComponentView) !== 0 : false;
    return Services.resolveDep(
        this.view, this.elDef, allowPrivateServices,
        {flags: DepFlags.None, token, tokenKey: tokenKey(token)}, notFoundValue);
  }
}
```

Basically every element can have merge injector even if you didn’t provide any token on it.

![=100%*](https://cdn-images-1.medium.com/max/2000/1*WGyA-RolH_Z68SMks4FmJA.gif)

