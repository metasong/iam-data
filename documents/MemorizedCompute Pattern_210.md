# MemorizedCompute Pattern
---
author: Jason Song <metaseed@gmail.com>
version: 1.1.0
tag: [pattern,memorize]
enable: [toc]
---

```ts
export function createMemoizedComputer(
  compute: AnyFn,
  isEqual = isEqualCheck
): MemoizedComputer {
  let lastArguments: null | IArguments;
  let lastResult: any;

  function reset() {
    lastArguments = undefined;
    lastResult = undefined;
  }

  function memoizedCompute(): any {
    if (!lastArguments) {
      lastResult = compute.apply(null, arguments);
      lastArguments = arguments;

      return lastResult;
    }

    for (let i = 0; i < arguments.length; i++) {
      if (!isEqual(arguments[i], lastArguments[i])) {
        lastResult = compute.apply(null, arguments);
        lastArguments = arguments;

        return lastResult;
      }
    }

    return lastResult;
  }

  return { memoizedCompute, reset };
}
```

```mermaid
sequenceDiagram
    participant C as Client
    participant F as ComputerFactory
    participant P as Computer
    C->>F:create(computeFn,isEqualFn)
    activate F
    F->>P:new()
    F-->>C:MemorizedComputer
    deactivate F
    C->>P:compute(parameterA)
    activate P
    P-->>C:result(A)
    deactivate P
    C->>P:compute(parameterA)
    P-->>C:directly return result(A)
    Note over C, P: compute with the same parameters directly return the same result
    C->>P:reset()
    activate P
    deactivate P
    
```
