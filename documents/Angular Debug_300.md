# Angular Debug
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---
test

> Note from:   
https://medium.com/front-end-hacking/a-guide-to-debugging-angular-applications-5a36bd88b4cf

## ng.probe($0).componentInstance
You can use this for *$1, $2, $3, $4* to inspect the previous four DOM elements.

## ng.profiler.timeChangeDetection()
to enable it in main.ts:  
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

to view the result, you could use `javascript Profiler` in Chrome Devtool:
![](https://cdn-images-1.medium.com/max/1000/1*iHoxgBHiGM85lZskVHjGtg.png)

## Angury
![](https://cdn-images-1.medium.com/max/1000/1*HyfbXNfZhGL-7kM7z9yBxQ.jpeg)

## Chrome Debugger
call stack and **restart frame**
![](https://cdn-images-1.medium.com/max/800/1*wOO5wM-0tsXxpNiouveIfw.gif)

conditional breakpoint: 
![](https://cdn-images-1.medium.com/max/800/1*IHNdZ56AOpUu46L9E-TIfg.gif)

