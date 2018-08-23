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

A **monad** is a way of composing functions that require context in addition to the return value, such as computation, branching, or I/O. Monads type lift, flatten and map so that the types line up for lifting functions `a => M(b)`, making 
them composable. It's a mapping from some type a to some type b along with some computational context, hidden in the implementation details of lift, flatten, and map:

* Functions map: a => b
* Functors map with context: Functor(a) => Functor(b)
* Monads flatten and map with context: Monad(Monad(a)) => Monad(b)

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
```js
  const compose = (f, g) => x => f(g(x));
  const x = 20;    // The value
  const arr = [x]; // The container
  // Some functions to compose
  const g = n => n + 1;
  const f = n => n * 2;
  // Proof that .map() accomplishes function composition.
  // Chaining calls to map is function composition.
  trace('map composes')([
    arr.map(g).map(f),
    arr.map(compose(f, g))
  ]);
  // => [42], [42]
}
const composePromises = composeM('then');
const composeMap = composeM('map');
const composeFlatMap = composeM('flatMap');
```
> *Performance Warning*: I’m not recommending this for arrays. Composing functions in this way would require multiple iterations over the entire array (which could contain hundreds of thousands of items). For maps over an array, compose simple a -> b functions first, then map over the array once, or optimize iterations with .reduce() or a transducer.

For synchronous, eager function applications over array data, this is overkill. However, lots of things are asynchronous or lazy, and lots of functions need to handle messy things like branching for exceptions or empty values.

That’s where monads come in. Monads can rely on values that depend on previous asynchronous or branching actions in the composition chain. In those cases, you can’t get a simple value out for simple function compositions. Your monad-returning actions take the form a => Monad(b) instead of a => b.

### The monad laws

1. Left identity: unit(x).chain(f) ==== f(x)
1. Right identity: m.chain(unit) ==== m
1. Associativity: m.chain(f).chain(g) ==== m.chain(x => f(x).chain(g)


![](https://cdn-images-1.medium.com/max/2000/1*X_bUJJYudP8MlhN0FLEGKg.png)

Homepage
JavaScript Scene
HOMELEARN JSVIDEO LESSONS
Go to the profile of Eric Elliott
Eric Elliott
Make some magic. #JavaScript
Sep 12, 2017
JavaScript Monads Made Simple
Smoke Art Cubes to Smoke — MattysFlicks — (CC BY 2.0)
Note: This is part of the “Composing Software” series on learning functional programming and compositional software techniques in JavaScript ES6+ from the ground up. Stay tuned. There’s a lot more of this to come!
< Previous | << Start over at Part 1
Before you begin to learn monads, you should already know:

Function composition: compose(f, g)(x) = (f ∘ g)(x) = f(g(x))
Functor basics: An understanding of the Array.map() operation.
“Once you understand monads, you immediately become incapable of explaining them to anyone else” Lady Monadgreen’s curse ~ Gilad Bracha (used famously by Douglas Crockford)
“Dr. Hoenikker used to say that any scientist who couldn’t explain to an eight-year-old what he was doing was a charlatan.” ~ Kurt Vonnegut’s novel Cat’s Cradle
If you go searching the internet for “monad” you’re going to get bombarded by impenetrable category theory math and a bunch of people “helpfully” explaining monads in terms of burritos and space suits.

Monads are simple. The lingo is hard. Let’s cut to the essence.

A monad is a way of composing functions that require context in addition to the return value, such as computation, branching, or I/O. Monads type lift, flatten and map so that the types line up for lifting functions a => M(b), making them composable. It's a mapping from some type a to some type b along with some computational context, hidden in the implementation details of lift, flatten, and map:

Functions map: a => b
Functors map with context: Functor(a) => Functor(b)
Monads flatten and map with context: Monad(Monad(a)) => Monad(b)
But what do “flatten” and “map” and “context” mean?

Map means, “apply a function to an a and return a b". Given some input, return some output.
Context is the computational detail of the monad’s composition (including lift, flatten, and map). The Functor/Monad API and its workings supply the context which allows you to compose the monad with the rest of the application. The point of functors and monads is to abstract that context away so we don’t have to worry about it while we’re composing things. Mapping inside the context means that you apply a function from a => b to the value inside the context, and return a new value b wrapped inside the same kind of context. Observables on the left? Observables on the right: Observable(a) => Observable(b). Arrays on the left side? Arrays on the right side: Array(a) => Array(b).
Type lift means to lift a type into a context, blessing the value with an API that you can use to compute from that value, trigger contextual computations, etc… a => F(a) (Monads are a kind of functor).
Flatten means unwrap the value from the context. F(a) => a.
Example:

const x = 20;             // Some data of type `a`
const f = n => n * 2;     // A function from `a` to `b`
const arr = Array.of(x);  // The type lift.
// JS has type lift sugar for arrays: [x]
// .map() applies the function f to the value x
// in the context of the array.
const result = arr.map(f); // [40]
In this case, Array is the context, and x is the value we're mapping over.

This example did not include arrays of arrays, but you can flatten arrays in JS with .concat():

[].concat.apply([], [[1], [2, 3], [4]]); // [1, 2, 3, 4]
You’re probably already using monads.
Regardless of your skill level or understanding of category theory, using monads makes your code easier to work with. Failing to take advantage of monads may make your code harder to work with (e.g., callback hell, nested conditional branches, more verbosity).

Remember, the essence of software development is composition, and monads make composition easier. Take another look at the essence of what monads are:

Functions map: a => b which lets you compose functions of type a => b
Functors map with context: Functor(a) => Functor(b), which lets you compose functions F(a) => F(b)
Monads flatten and map with context: Monad(Monad(a)) => Monad(b), which lets you compose lifting functions a => F(b)
These are all just different ways of expressing function composition. The whole reason functions exist is so you can compose them. Functions help you break down complex problems into simple problems that are easier to solve in isolation, so that you can compose them in various ways to form your application.

The key to understanding functions and their proper use is a deeper understanding of function composition.

Function composition creates function pipelines that your data flows through. You put some input in the first stage of the pipeline, and some data pops out of the last stage of the pipeline, transformed. But for that to work, each stage of the pipeline must be expecting the data type that the previous stage returns.

Composing simple functions is easy, because the types all line up easily. Just match output type b to input type b and you're in business:

g:           a => b
f:                b => c
h = f(g(a)): a    =>   c
Composing with functors is also easy if you’re mapping F(a) => F(b) because the types line up:

g:             F(a) => F(b)
f:                     F(b) => F(c)
h = f(g(Fa)):  F(a)    =>      F(c)
But if you want to compose functions from a => F(b), b => F(c), and so on, you need monads. Let's swap the F() for M() to make that clear:

g:                  a => M(b)
f:                       b => M(c)
h = composeM(f, g): a    =>   M(c)
Oops. In this example, the component function types don’t line up! For f's input, we wanted type b, but what we got was type M(b) (a monad of b). Because of that misalignment, composeM() needs to unwrap the M(b) that g returns so we can pass it to f, because f is expecting type b, not type M(b). That process (often called .bind() or .chain()) is where flatten and map happen.

It unwraps the b from M(b) before passing it to the next function, which leads to this:

g:             a => M(b) flattens to => b
f:                                      b           maps to => M(c)
h composeM(f, g):
               a       flatten(M(b)) => b => map(b => M(c)) => M(c)
Monads make the types line up for lifting functions a => M(b), so that you can compose them.

In the above diagram, the flatten from M(b) => b and the map from b => M(c) happens inside the chain from a => M(c). The chain invocation is handled inside composeM(). At a high level, you don't have to worry about it. You can just compose monad-returning functions using the same kind of API you'd use to compose normal functions.

Monads are needed because lots of functions aren’t simple mappings from a => b. Some functions need to deal with side effects (promises, streams), handle branching (Maybe), deal with exceptions (Either), etc...

Here’s a more concrete example. What if you need to fetch a user from an asynchronous API, and then pass that user data to another asynchronous API to perform some calculation?:

getUserById(id: String) => Promise(User)
hasPermision(User) => Promise(Boolean)
Let’s write some functions to demonstrate the problem. First, the utilities, compose() and trace():

const compose = (...fns) => x => fns.reduceRight((y, f) => f(y), x);
const trace = label => value => {
  console.log(`${ label }: ${ value }`);
  return value;
};
Then some functions to compose:

{
  const label = 'API call composition';
  // a => Promise(b)
  const getUserById = id => id === 3 ?
    Promise.resolve({ name: 'Kurt', role: 'Author' }) :
    undefined
  ;
  // b => Promise(c)
  const hasPermission = ({ role }) => (
    Promise.resolve(role === 'Author')
  );
  // Try to compose them. Warning: this will fail.
  const authUser = compose(hasPermission, getUserById);
  // Oops! Always false!
  authUser(3).then(trace(label));
}
When we try to compose hasPermission() with getUserById() to form authUser() we run into a big problem because hasPermission() is expecting a User object and getting a Promise(User) instead. To fix this, we need to swap out compose() for composePromises() — a special version of compose that knows it needs to use .then() to accomplish the function composition:

{
  const composeM = chainMethod => (...ms) => (
    ms.reduce((f, g) => x => g(x)[chainMethod](f))
  );
  const composePromises = composeM('then');
  const label = 'API call composition';
  // a => Promise(b)
  const getUserById = id => id === 3 ?
    Promise.resolve({ name: 'Kurt', role: 'Author' }) :
    undefined
  ;
  // b => Promise(c)
  const hasPermission = ({ role }) => (
    Promise.resolve(role === 'Author')
  );
  // Compose the functions (this works!)
  const authUser = composePromises(hasPermission, getUserById);
  authUser(3).then(trace(label)); // true
}
We’ll get into what composeM() is doing, later.

Remember the essence of monads:

Functions map: a => b
Functors map with context: Functor(a) => Functor(b)
Monads flatten and map with context: Monad(Monad(a)) => Monad(b)
In this case, our monads are really promises, so when we compose these promise-returning functions, we have a Promise(User) instead of the User that hasPermission() is expecting. Notice that if you took the outer Monad() wrapper off of Monad(Monad(a)), you'd be left with Monad(a) => Monad(b), which is just the regular functor .map(). If we had something that could flatten Monad(x) => x, we'd be in business.

What Monads are Made of
A monad is based on a simple symmetry — A way to wrap a value into a context, and a way to unwrap the value from the context:

Lift/Unit: A type lift from some type into the monad context: a => M(a)
Flatten/Join: Unwrapping the type from the context: M(a) => a
And since monads are also functors, they can also map:

Map: Map with context preserved: M(a) -> M(b)
Combine flatten with map, and you get chain — function composition for monad-lifting functions, aka Kleisli composition, named after Heinrich Kleisli:

FlatMap/Chain: Flatten + map: M(M(a)) => M(b)
For monads, .map() methods are often omitted from the public API. Lift + flatten don't explicitly spell out .map(), but you have all the ingredients you need to make it. If you can lift (aka of/unit) and chain (aka bind/flatMap), you can make .map():

const MyMonad = value => ({
  // <... insert arbitrary chain and of here ...>
  map (f) {
    return this.chain(a => this.constructor.of(f(a)));
  }
});
So, if you define .of() and .chain()/.join() for your monad, you can infer the definition of .map().

The lift is the factory/constructor and/or constructor.of() method. In category theory, it's called “unit”. All it does is lift the type into the context of the monad. It turns an a into a Monad of a.

In Haskell, it’s (very confusingly) called return, which gets extremely confusing when you try to talk about it out-loud because nearly everyone confuses it with function returns. I almost always call it "lift" or "type lift" in prose, and .of() in code.

That flattening process (without the map in .chain()) is usually called flatten() or join(). Frequently (but not always), flatten()/join() is omitted completely because it's built into .chain()/.flatMap(). Flattening is often associated with composition, so it's frequently combined with mapping. Remember, unwrapping + map are both needed to compose a => M(a) functions.

Depending on what kind of monad you’re dealing with, the unwrapping process could be extremely simple. In the case of the identity monad, it’s just like .map(), except that you don't lift the resulting value back into the monad context. That has the effect of discarding one layer of wrapping:

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
But the unwrapping part is also where the weird stuff like side effects, error branching, or waiting for async I/O typically hides. In all software development, composition is where all the real interesting stuff happens.

For example, with promises, .chain() called .then(). Calling promise.then(f) won't invoke f() right away. Instead, it will wait for the promise to resolve, and then call f() (hence the name).

Example:

{
  const x = 20;                 // The value
  const p = Promise.resolve(x); // The context
  const f = n => 
    Promise.resolve(n * 2);     // The function
  const result = p.then(f);     // The application
  result.then(
    r => console.log(r)         // 40
  );
}
With promises, .then() is used instead of .chain(), but it's almost the same thing.

You may have heard that a promise is not strictly a monad. That’s because it will only unwrap the outer promise if the value is a promise to begin with. Otherwise, .then() behaves like .map().

But because it behaves differently for promise values and other values, .then() does not strictly obey all the mathematical laws that all functors and/or monads must satisfy for all given values. In practice, as long as you're aware of that behavior branching, you can usually treat them as either. Just be aware that some generic composition tools may not work as expected with promises.

Building monadic (aka Kleisli) composition
Let’s take a deeper look at the composeM function we used to compose promise-lifting functions:

const composeM = method => (...ms) => (
  ms.reduce((f, g) => x => g(x)[method](f))
);
Hidden in that weird reducer is the algebraic definition of function composition: f(g(x)). Let's make it easier to spot:

{
  // The algebraic definition of function composition:
  // (f ∘ g)(x) = f(g(x))
  const compose = (f, g) => x => f(g(x));
  const x = 20;    // The value
  const arr = [x]; // The container
  // Some functions to compose
  const g = n => n + 1;
  const f = n => n * 2;
  // Proof that .map() accomplishes function composition.
  // Chaining calls to map is function composition.
  trace('map composes')([
    arr.map(g).map(f),
    arr.map(compose(f, g))
  ]);
  // => [42], [42]
}
What this means is that we could write a generalized compose utility that should work for all functors which supply a .map()method (e.g., arrays):

const composeMap = (...ms) => (
  ms.reduce((f, g) => x => g(x).map(f))
);
This is just a slight reformulation of the standard f(g(x)). Given any number of functions of type a -> Functor(b), iterate through each function and apply each one to its input value, x. The .reduce() method takes a function with two input values: An accumulator (f in this case), and the current item in the array (g).

We return a new function x => g(x).map(f) which becomes f in the next application. We've already proved above that x => g(x).map(f) is equivalent to lifting compose(f, g)(x) into the context of the functor. In other words, it's equivalent to applying f(g(x)) to the values in the container: In this case, that would apply the composition to the values inside the array.

Performance Warning: I’m not recommending this for arrays. Composing functions in this way would require multiple iterations over the entire array (which could contain hundreds of thousands of items). For maps over an array, compose simple a -> b functions first, then map over the array once, or optimize iterations with .reduce() or a transducer.
For synchronous, eager function applications over array data, this is overkill. However, lots of things are asynchronous or lazy, and lots of functions need to handle messy things like branching for exceptions or empty values.

That’s where monads come in. Monads can rely on values that depend on previous asynchronous or branching actions in the composition chain. In those cases, you can’t get a simple value out for simple function compositions. Your monad-returning actions take the form a => Monad(b) instead of a => b.

Whenever you have a function that takes some data, hits an API, and returns a corresponding value, and another function that takes that data, hits another API, and returns the result of a computation on that data, you’ll want to compose functions of type a => Monad(b). Because the API calls are asynchronous, you'll need to wrap the return values in something like a promise or observable. In other words, the signatures for those functions are a -> Monad(b), and b -> Monad(c), respectively.

Composing functions of type g: a -> b, f: b -> c is easy because the types line up: h: a -> c is just a => f(g(a)).

Composing functions of type g: a -> Monad(b), f: b -> Monad(c) is a little harder: h: a -> Monad(c) is not just a => f(g(a)) because f is expecting b, not Monad(b).

Let’s get a little more concrete and compose a pair of asynchronous functions that each return a promise:

{
  const label = 'Promise composition';
  const g = n => Promise.resolve(n + 1);
  const f = n => Promise.resolve(n * 2);
  const h = composePromises(f, g);
  h(20)
    .then(trace(label))
  ;
  // Promise composition: 42
}
How do we write composePromises() so that the result is logged correctly? Hint: You've already seen it.

Remember our composeMap() function? All you need to do is change the .map() call to .then(). Promise.then() is basically an asynchronous .map().

{
  const composePromises = (...ms) => (
    ms.reduce((f, g) => x => g(x).then(f))
  );
  const label = 'Promise composition';
  const g = n => Promise.resolve(n + 1);
  const f = n => Promise.resolve(n * 2);
  const h = composePromises(f, g);
  h(20)
    .then(trace(label))
  ;
  // Promise composition: 42
}
The weird part is that when you hit the second function, f (remember, f after g), the input value is a promise. It's not type b, it's type Promise(b), but f takes type b, unwrapped. So what's going on?

Inside .then(), there's an unwrapping process that goes from Promise(b) -> b. That operation is called join or flatten.

You may have noticed that composeMap() and composePromises() are almost identical functions. This is the perfect use-case for a higher-order function that can handle both. Let's just mix the chain method into a curried function, then use square bracket notation:

const composeM = method => (...ms) => (
  ms.reduce((f, g) => x => g(x)[method](f))
);
Now we can write the specialized implementations like this:

const composePromises = composeM('then');
const composeMap = composeM('map');
const composeFlatMap = composeM('flatMap');
The monad laws
Before you can start building your own monads, you need to know there are three laws that all monads should satisfy:

Left identity: unit(x).chain(f) ==== f(x)
Right identity: m.chain(unit) ==== m
Associativity: m.chain(f).chain(g) ==== m.chain(x => f(x).chain(g))
The Identity Laws

Left and right identity
A monad is a functor. A functor is a morphism between categories, A -> B. The morphism is represented by an arrow. In addition to the arrow we explicitly see between objects, each object in a category also has an arrow back to itself. In other words, for every object X in a category, there exists an arrow X -> X. That arrow is known as the identity arrow

![]

### Proving the Monad Laws

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
  const g = n => Id(n + 1);
  const f = n => Id(n * 2);
  // Left identity
  // unit(x).chain(f) ==== f(x)
  trace('Id monad left identity')([
    Id(x).chain(f),
    f(x)
  ]);
  // Id monad left identity: Id(40), Id(40)

  // Right identity
  // m.chain(unit) ==== m
  trace('Id monad right identity')([
    Id(x).chain(Id.of),
    Id(x)
  ]);
  // Id monad right identity: Id(20), Id(20)
  // Associativity
  // m.chain(f).chain(g) ====
  // m.chain(x => f(x).chain(g)  
  trace('Id monad associativity')([
    Id(x).chain(g).chain(f),
    Id(x).chain(x => g(x).chain(f))
  ]);
  // Id monad associativity: Id(42), Id(42)
}
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
  (conditionA)?
    (conditionB)?
       valueA
       :valueB
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
  (!conditionA)?
    valueC
    :(conditionB)?
        valueA
        :valueB
);
```
> If you get to the end of a ternary expression and find you need to write two colon clauses`:`, grab the last clause, move it to the top, and reverse the logic of the first conditional to simplify parsing the ternary. No more confusion!


