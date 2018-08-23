# Composing Software 1

## [Functional mixins](https://medium.com/javascript-scene/functional-mixins-composing-software-ffb66d5e731c)
function mixins are composable factory functions which connect together in a pipeline; each function adding some properties or behaviors like workers on an assembly line. Functional mixins don’t depend on or require a base factory or constructor: Simply pass any arbitrary object into a mixin, and an enhanced version of that object will be returned.
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

```js
const withConstructor = constructor => o => {
  const proto = Object.assign({},
    Object.getPrototypeOf(o),
    { constructor }
  );
  return Object.assign(Object.create(proto), o);
};
const withFlying = o => {
  let isFlying = false;
  return {
    ...o,
    fly () {
      isFlying = true;
      return this;
    },
    land () {
      isFlying = false;
      return this;
    },
    isFlying: () => isFlying
  }
};

const withBattery = ({ capacity }) => o => {
  let percentCharged = 100;
  return {
    ...o,
    draw (percent) {
      const remaining = percentCharged - percent;
      percentCharged = remaining > 0 ? remaining : 0;
      return this;
    },
    getCharge: () => percentCharged,
    get capacity () {
      return capacity
    }
  };
};
const createDrone = ({ capacity = '3000mAh' }) => pipe(
  withFlying,
  withBattery({ capacity }),
  withConstructor(createDrone)
)({});
const myDrone = createDrone({ capacity: '5500mAh' });
console.log(`
  can fly:  ${ myDrone.fly().isFlying() === true }
  can land: ${ myDrone.land().isFlying() === false }
  battery capacity: ${ myDrone.capacity }
  battery status: ${ myDrone.draw(50).getCharge() }%
  battery drained: ${ myDrone.draw(75).getCharge() }%
`);
console.log(`
  constructor linked: ${ myDrone.constructor === createDrone }
`);
```
withBattery() could be used with other kinds of objects, like robots, electric skateboards, or portable device chargers. withFlying() could be used to model flying cars, rockets, or air balloons.

## [Why Composition is Harder with Classes](https://medium.com/javascript-scene/why-composition-is-harder-with-classes-c3e627dcd0aa)
### what does new actually do?
* Creates a new object and binds this to it in the constructor function.
* Implicitly returns this, unless you explicitly return another object.
* Sets the instance [[Prototype]] (an internal reference) to Constructor.prototype, so that Object.getPrototypeOf(instance) === Constructor.prototype.
* Sets the instance.constructor === Constructor.
## [Composable Datatypes with Functions](https://medium.com/javascript-scene/composable-datatypes-with-functions-aec72db3b093)
```js
const t = value => {
  const add = n => t(value + n);
  return Object.assign(add, {
    toString: () => `t(${ value })`,
    valueOf: () => value
  });
};

const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);
// Sugar to kick off the pipeline with an initial value:
const sumT = (...fns) => pipe(...fns)(t(0));
sumT(
  t(2),
  t(4),
  t(-1)
).valueOf(); // 5
```
## [JavaScript Monads](https://medium.com/javascript-scene/javascript-monads-made-simple-7856be57bfe8)
```js
const trace = label => value => {
  console.log(`${ label }: ${ value }`);
  return value;
};

{
  const composeM = chainMethod => (...ms) => (
    ms.reduce((f, g) => x => g(x)[chainMethod](f))
  );
  
  const composePromises = composeM('then');
  const label = 'API call composition';
  // a => Promise(b)
  const getUserById = id => id === 3 ?
    Promise.resolve({ name: 'Kurt', role: 'Author' }) :
    Promise.resolve({})
  ;
  // b => Promise(c)
  const hasPermission = ({ role }) => (
    Promise.resolve(role === 'Author')
  );
  // Compose the functions (this works!)
  const authUser = composePromises(hasPermission, getUserById);
  authUser(3).then(trace(label)); // true
}
```
### What Monads are Made of
> A monad is based on a simple symmetry — A way to wrap a value into a context, and a way to unwrap the value from the context:

* **Lift/Unit**: A type lift from some type into the monad context: a => M(a)
* **Flatten/Join**: Unwrapping the type from the context: M(a) => a
* And since monads are also functors, they can also map:   
**Map**: Map with context preserved: M(a) -> M(b)

  Combine flatten with map, and you get chain — function composition for monad-lifting functions, aka Kleisli composition, named after *Heinrich Kleisli*

* **FlatMap/Chain**: Flatten + map: M(M(a)) => M(b)

```js
{ // Identity monad
const Id = value => ({
  // Functor mapping
  // Preserve the wrapping for .map() by 
  // passing the mapped value into the type
  // lift:
  map: f => Id.of(f(value)),
  // Monad chaining
  // Discard one level of wrapping
  // by omitting the .of() type lift:
  chain: f => f(value),
  // Just a convenient way to inspect
  // the values:
  toString: () => `Id(${ value })`
});
// The type lift for this monad is just
// a reference to the factory.
Id.of = Id;
```

## [Nested Ternaries are Great](https://medium.com/javascript-scene/nested-ternaries-are-great-361bddd0f340)
### Expressions vs Statements
> pression is a chunk of code that evaluates to a single value.
> A statement is a chunk of code that may not evaluate to a value at all.

In JavaScript if statements don’t evaluate to values. In order for an if statement in JavaScript to do anything useful, it must cause a side-effect or return a value from the containing function.

```js
const withIf = ({
  conditionA, conditionB
}) => {
  if (conditionA) {
    if (conditionB) {
      return valueA;
    }
    return valueB;
  }
  return valueC;
};

const withTernary = ({
  conditionA, conditionB
}) => (
  (conditionA)
    ? (conditionB)
        ? valueA
        : valueB
    : valueC
);

const withIf = ({
  conditionA, conditionB
}) => {
  if (!conditionA) 
    return value C;
  if (conditionB) {
      return valueA;
    }
    return valueB;
};

const withTernary = ({
  conditionA, conditionB
}) => (
  (!conditionA)
    ? valueC
    : (conditionB)
        ? valueA
        : valueB
);
```
If you get to the end of a ternary expression and find you need to write two colon clauses (:), grab the last clause, move it to the top, and reverse the logic of the first conditional to simplify parsing the ternary. No more confusion!


