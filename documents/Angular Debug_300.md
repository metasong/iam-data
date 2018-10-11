# Angular Debug
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---

> Note from:   
https://medium.com/front-end-hacking/a-guide-to-debugging-angular-applications-5a36bd88b4cf

## ng.probe($0).componentInstance
You can use this for *$1, $2, $3, $4* to inspect the previous four DOM elements.

## ng.profiler.timeChangeDetection()
to enable it:  
```js {5-7}
const bootstrap = platformBrowserDynamic().bootstrapModule(AppModule);

if (!environment.production) {
  bootstrap.then(moduleRef => {
    const applicationRef = moduleRef.injector.get(ApplicationRef);
    const appComponent = applicationRef.components[0];
    enableDebugTools(appComponent);
  });
}
```
