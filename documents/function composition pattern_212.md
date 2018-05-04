# function composition pattern

```ts
export function compose<A, B, C, D, E, F>(
  f: (i: E) => F,
  e: (i: D) => E,
  d: (i: C) => D,
  c: (i: B) => C,
  b: (i: A) => B
): (i: A) => F;
export function compose<A = any, F = any>(...functions: any[]): (i: A) => F;
export function compose(...functions: any[]) {
  return function(arg: any) {
    return functions.reduceRight((composed, fn) => fn(composed), arg);
  };
}
```
