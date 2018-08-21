# [Composing Software 1](
https://medium.com/javascript-scene/functional-mixins-composing-software-ffb66d5e731c)
**Functional mixins** are composable factory functions which connect together in a pipeline; each function adding some properties or behaviors like workers on an assembly line. Functional mixins don’t depend on or require a base factory or constructor: Simply pass any arbitrary object into a mixin, and an enhanced version of that object will be returned.
### Functional mixin features:

* Data privacy/encapsulation
* Inheriting private state
* Inheriting from multiple sources
* No diamond problem (property collision ambiguity) — last in wins
* No base-class requirement

```js
const quacking = quack => o => Object.assign({}, o, {
  quack: () => quack
});
const flying = o => {
  let isFlying = false;
  return Object.assign({}, o, {
    fly () {
      isFlying = true;
      return this;
    },
    isFlying: () => isFlying,
    land () {
      isFlying = false;
      return this;
    }
  });
};

const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);
// OR...
// import pipe from `lodash/fp/flow`;
const createDuck = quack => pipe(
  flying,
  quacking(quack)
)({});
const duck = createDuck('Quack!');
console.log(duck.fly().quack());



```

```js
// in its own module...
const withLogging = logger => o => Object.assign({}, o, {
  log (text) {
    logger(text)
  }
});

// in a different module
const withConfig = config => (o = {
  log: (text = '') => console.log(text)
}) => Object.assign({}, o, {
  get (key) {
    return config[key] == undefined ?
      this.log(`Missing config key: ${ key }`) :
      config[key]
    ;
  }
});
// in yet another module that imports withLogging and
// withConfig...
const createConfig = ({ initialConfig, logger }) =>
  pipe(
    withLogging(logger),
    withConfig(initialConfig)
  )({})
;
// elsewhere...
const initialConfig = {
  host: 'localhost'
};
const logger = console.log.bind(console);
const config = createConfig({initialConfig, logger});
console.log(config.get('host')); // 'localhost'
config.get('notThere'); // 'Missing config key: notThere
```
