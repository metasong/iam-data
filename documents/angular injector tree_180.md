# angular injector tree
*summery*
https://blog.angularindepth.com/angular-dependency-injection-and-tree-shakeable-tokens-4588a8f70d5d

![=100%*](https://cdn-images-1.medium.com/max/2000/1*rjG7U4vLG_keRYoZnryxbA.png)

* MyService2 from EagerModule2 will be included into the root injector.
* `ComponentFactoryResolver` is also [added](https://github.com/angular/angular/blob/f258ec67bf20974362c33f59ec2be79fa9f8b9c8/packages/compiler/src/ng_module_compiler.ts#L36) to the root module injector by Angular. This resolver is responsible for _dynamic_ creation components since it stores factories of `entryComponents`.
*  Module Tokens are actually types of all merged NgModules.