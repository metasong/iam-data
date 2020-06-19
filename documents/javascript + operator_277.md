# javascript + operator
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: [javascript, +, add]
enable: [toc]
---

There are two kinds of values in JavaScript: primitives and objects. The primitive values are: undefined, null, booleans, numbers, and strings. All other values are objects, including arrays and functions.

The plus operator performs three kinds of conversion: It converts values to primitives, numbers and strings:
## Converting Values ((p)9.1)
### Object 'ToPrimitive(input, PreferredType?)'
The optional parameter PreferredType is either `Number` or `String`. It only expresses a preference, the result can always be any primitive value. If PreferredType is `Number`, the following steps are performed to convert a value input (§9.1):
1. If input is primitive, return it as is. 
1. Otherwise, input is an object. Call obj.valueOf(). If the result is primitive, return it.
1. Otherwise, call obj.toString(). If the result is a primitive, return it.
1. Otherwise, throw a TypeError.

> If PreferredType is `String`, steps 2 and 3 are swapped. If PreferredType is missing then it is set to `String` for instances of `Date` and to `Number` for all other values.

### Primitives 'ToNumber()' ((p)9.3)
|Argument	|Result|
|---|---|
undefined|	NaN
null	|+0
boolean value|	true is converted to 1, false is converted to +0
number value|	no conversion necessary
string value |parse the number in the string. For example, "324" is converted to 324

> An object obj is converted to a number by calling ToPrimitive(obj, Number) and then applying ToNumber() to the (primitive) result.

### Primitives to 'ToString()' ((p)9.8)

Argument|	Result
---|---
undefined|	"undefined"
null	|"null"
boolean value|	either "true" or "false"
number value|	the number as a string, e.g. "1.765"
string value	|no conversion necessary

> An object obj is converted to a number by calling ToPrimitive(obj, String) and then applying ToString() to the (primitive) result.
## Try it out
```js
    var obj = {
        valueOf: function () {
            console.log("valueOf");
            return {}; // not a primitive
        },
        toString: function () {
            console.log("toString");
            return {}; // not a primitive
        }
    }
```

## References
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators
http://2ality.com/2012/01/object-plus-object.html

adfadd ad adf dad fa

