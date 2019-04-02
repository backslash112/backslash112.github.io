---
layout: post
title:  "'this', '__proto__' and 'prototype' in JavaScript"
date:  2019-04-02
description: ""
img: js.jpg
tags: [js]
---

## How does the `this` keyword work?

### ThisBinding
 If a function is called on an object, such as in obj.myMethod() or the equivalent obj["myMethod"](), then ThisBinding is set to the object (obj in the example). 
 In most other cases, ThisBinding is set to the global object.


### Build-in Special Functions
 The reason for writing "in most other cases" is because there are eight ECMAScript 5 built-in functions that allow ThisBinding to be specified in the arguments list. These special functions take a so-called thisArg which becomes the ThisBinding when calling the function.

- `Function.prototype.apply( thisArg, argArray )`
- `Function.prototype.call( thisArg [ , arg1 [ , arg2, ... ] ] )`
- `Function.prototype.bind( thisArg [ , arg1 [ , arg2, ... ] ] )`
- `Array.prototype.every( callbackfn [ , thisArg ] )`
- `Array.prototype.some( callbackfn [ , thisArg ] )`
- `Array.prototype.forEach( callbackfn [ , thisArg ] )`
- `Array.prototype.map( callbackfn [ , thisArg ] )`
- `Array.prototype.filter( callbackfn [ , thisArg ] )`

### Arrow Functions: `() => {}`
Arrow functions (introduced in ECMA6) alter the scope of this.
Arrow functions don't have their own this.... binding.

### When invoking context-less function
When you use this inside function that is invoked without any context (i.e. not on any object), it is bound to the global object (window in browser)(even if the function is defined inside the object).
```javascript
var context = "global";

var obj = {  
    context: "object",
    method: function () {                  
        function f() {
            var context = "function";
            return this + ":" +this.context; 
        };
        return f(); //invoked without context
    }
};
document.write(obj.method()); //[object Window]:global 
```

## `__proto__` and `prototype`
- A function is an object in JavaScript.
- EVERY object in JavaScript gets an internal property: `__proto__`.(
all objects in JavaScript have an internal `__proto__` property which points to something.)
- JavaScript creates a property called `prototype` for a function when you define the function. On each function definition there's a new property created called `prototype`.

`A.prototype` is **TOTALLY DIFFERENT** from the `__proto__` property.

#### `__proto__`

So what does this `__proto__` property points to? 
- Well, usually **another object**.

Why does JavaScript has `__proto__` property created on every single object? 

- Well, one word: **delegation**.
(When you call a property on an object and the object doesn't have it, then JavaScript looks for the object referenced by `__proto__` to see if it maybe has it. If it doesn't have it, then it looks at that object's `__proto__` property and so on...until the chain ends. Thus the name *prototype chain*.)

#### `prototype`

Why does JavaScript creates a property called `prototype` for a function when you define the function? 
- **Because it tries to fool you, yes fool you that it works like class-based languages.** 

```javascript
var a1 = new A();
```
1. What the new operator does is that it sets that __proto__ property to point to the function's prototype property.

2. We said that `A.prototype` is nothing more than an **empty object**.

## References
- https://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work
- https://stackoverflow.com/a/3127440/2195426
- https://stackoverflow.com/a/17514482/2195426
- https://stackoverflow.com/questions/80084/in-javascript-why-is-the-this-operator-inconsistent