# Functional Programming - Transducer
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: [javascript]
enable: [toc]
---
> Transduce: Derived from the 17th century scientific latin, “transductionem” means “to change over, convert”. It is further derived from “transducere/traducere”, which means “to lead along or across, transfer”.

**transducer**  (sometimes referred to as xform or xf) is a composable higher order reducer. It takes a reducer as input, and returns another
reducer.
* Composable using simple function composition
* Efficient for large collections or infinite streams: Only enumerates over the elements once, regardless of the number of operations in the pipeline
* Able to transduce over any enumerable source (e.g., arrays, trees, streams, graphs, etc…)
* Usable for either lazy or eager evaluation with no changes to the transducer pipeline; Transducers don’t care whether you pull or push. Transducers have no awareness of the data structure they’re acting on. They simply call the reducer you pass into them to accumulate new values.

Reducers fold multiple inputs into single outputs, where “fold” can be replaced with virtually any
binary operation that produces a single output.

Normal reducers can’t compose, because they expect two arguments, and only return a single output
value, so you can’t simply conne

**reducer**  
![=400*](https://www.electrical4u.com/electrical/wp-content/uploads/2014/04/closed-loop-control-system-11-3-15.gif)
```js
// reducer<A,C> = (accumulator, current) => accumulator
// transducer = reducer => reducer
// special case: if the f take type a and return type a, it meets the requirement of compose
// actual: f3<-f2<-f1.
// Func<T>: T=>T
// compose: (Func<Y,Z>,Func<X,Y>...) => Func<X,Z> // f3(f2(f1(A)))->C
// Enumerable<C>.reduce: (reducer<A,C>,A)=>A
const composeNormal = (...fns) => a => fns.reduceRight((acc, f) => f(acc), a);
// so it can be used to take transducers(t:r=>r) and then pass in a reducer as initial value.
// tds: transducers; t: transducer; r: reducer; ar: accumulated reducer
// compose transducers return a transducer(reducer=>reducer)
const compose = (...tds) => r => tds.reduceRight((ar, t) => t(ar), r);

// map: f=> reducer => reducer 
// map(f) return a transducer
// step is reducer

// a map transducer takes a mapping function (called a transform) and a reducer (called
// the step function), and returns a new reducer. 
// The step function is a reducer to call when we’ve
// produced a new value to be used as the accumulator in the next step.
//tf:transform
const map = tf => step => // step is previous step
// 'a':result from last executed step, here the filter is last executed step, although it's the next step of map in the `compose` function
// executed-steps are the reversing of the definited composed-steps 
  (a, c) => step(a, tf(c)); // in this example, it's arrayConct
// filter(p) return a transducer
const filter = predicate => step =>
  (a, c) => predicate(c) ? step(a, c) : a;
// if
// const filter = predicate => step => (a, c) => {
// last=step(a, c);
// WRONG!!: because last is an array in this example
// predicate(last) ? last : a;}
// below should be pipe vs compose

// the right explanation for the reverse is:
// we call last step in current step processing
const isEven = n => n % 2 === 0;
const double = n => n * 2;
const doubledEvens = compose(
  filter(isEven),
  map(double)
); // transducer

// is the first composed-step
// is the last executed-step of the reducers
// defines the output of the last executed-step
// in tranducer only the last-executed step uses the 'a' to procuce result, others just pass 'a' into next execute-step, so xform definites the output of transducers
// supplies the knowledge of how to accumulate the result
const arrayConcat = (a, c) => a.concat([c]); // reducer

// xform: transform.
const xform = doubledEvens(arrayConcat); //reducer
const result = [1,2,3,4,5,6].reduce(xform, []); // [4, 8, 12]
console.log(result);
```
> You can think of transducers like signal processing gear (e.g., guitar distortion pedals,
EQ, volume knobs, echo, reverb, and audio mixers):  
> `[ Source ] -> [ Mic ] -> [ Filter ] -> [ Mixer ] -> [ Recording ]`  
> `[ Enumerator ] -> [ Transducer ] -> [ Transducer ] -> [ Accumulator]`

## Transducer Rules

1. Initialization: Given no initial accumulator value, a transducer must call the step function to produce a valid initial value to act on. The value should represent the empty state. For example, an accumulator that accumulates an array should supply an empty array when its step function is called with no arguments.
1. Early termination: A process that uses transducers must check for and stop when it receives a reduced accumulator value. Additionally, a transducer step function that uses a nested reduce must check for and convey reduced values when they are encountered.
1. Completion (optional): Some transducing processes never complete, but those that do should call the completion function to produce a final value and/or flush state, and stateful transducers should supply a completion operation that cleans up any accumulated resources and potentially produces one final value.

```js
// import a standard curry, or use this magic spell:
const curry = (
  f, arr = []
) => (...args) => (
  a => a.length === f.length ?
    f(...a) :
    curry(f, a)
)([...arr, ...args]);
const transduce = curry((step, initial, xform, foldable) =>
  foldable.reduce(xform(step), initial)
);

```
## Example from Ramda

```js
import {
  compose,
  filter,
  map,
  into
} from 'ramda';
const isEven = n => n % 2 === 0;
const double = n => n * 2;
const doubleEvens = compose(
  filter(isEven),
  map(double)
);
const arr = [1, 2, 3, 4, 5, 6];
// into: (structure, transducer, data) => result
// into transduces the data using the supplied
// transducer into the structure passed as the
// first argument.
const result = into([], doubleEvens, arr);
console.log(result); // [4, 8, 12]
```

https://medium.com/javascript-scene/transducers-efficient-data-processing-pipelines-in-javascript-7985330fe73d


