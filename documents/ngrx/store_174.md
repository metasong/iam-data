# ngrx/store
[[TOC]]

[note from Comprehensive introduction to @ngrx/store](https://gist.github.com/btroncone/a6e4347326749f938510)

your store can be thought of as a client side ‘single source of truth’, or database. 

**reducer interface**
```ts
export interface Reducer<T> {
  (state: T, action: Action): T;
}
```
