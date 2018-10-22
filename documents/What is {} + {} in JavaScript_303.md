# What is {} + {} in JavaScript
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---

http://2ality.com/2012/01/object-plus-object.html

The general rule for addition in JavaScript is simple: You can only add numbers and strings, all other values will be converted to either one of those types. 

## Converting values to primitives via ToPrimitive
```js
ToPrimitive(input, PreferredType?)
```

If PreferredType is Number:
1. If input is primitive, return it as is.
1. Otherwise, input is an object. Call obj.valueOf(). If the result is primitive, return it.
Otherwise, call obj.toString(). If the result is a primitive, return it.
Otherwise, throw a TypeError.


