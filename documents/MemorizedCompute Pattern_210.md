# MemorizedCompute Pattern

```ts
export function createMemoizedComputor(
  compute: AnyFn,
  isEqual = isEqualCheck
): MemoizedComputor {
  let lastArguments: null | IArguments = null;
  let lastResult: any = null;

  function reset() {
    lastArguments = null;
    lastResult = null;
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
    participant F as MemorizedComputorFactory
    participant P as MemorizedComputer
    C->>F:create(computeFn,isEqual)
    F->>P:new()
    F-->>C:MemorizedComputer
    C->>P:compute(parameterA)
    P->>C:result(A)
    C->>P:compute(parameterA)
    P->>C:directly return result(A)
    C->>MemorizedComputer:reset()
    
```
