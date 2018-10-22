# What is {} + {} in JavaScript
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---

http://2ality.com/2012/01/object-plus-object.html

The general rule for addition in JavaScript is simple: You can only add numbers and strings, all other values will be converted to either one of those types.

## Addition
```js
value1 + value2
```
1. Convert both operands to primitives (mathematical notation, not JavaScript):    
       ```
       prim1 := ToPrimitive(value1)
       prim2 := ToPrimitive(value2)
       ```
1. PreferredType is omitted and thus Number for non-dates, String for dates.
If either prim1 or prim2 is a string then convert both to strings and return the concatenation of the results.
Otherwise, convert both prim1 and prim2 to numbers and return the sum of the results.

## Converting

### Converting values to primitives via ToPrimitive
```js
ToPrimitive(input, PreferredType?)
```

If PreferredType is Number:
1. If input is primitive, return it as is.
1. Otherwise, input is an object. Call obj.valueOf(). If the result is primitive, return it.
1. Otherwise, call obj.toString(). If the result is a primitive, return it.
1. Otherwise, throw a TypeError.

If PreferredType is String, steps 2 and 3 are swapped. If PreferredType is missing then it is set to String for instances of Date and to Number for all other values.



