---
layout: post
title:  "'this', '__proto__' and 'prototype' in JavaScript"
date:  2019-04-02
description: ""
img: js1.png
tags: [js]
---
## How do JavaScript closures work? ([stackoverflow](https://stackoverflow.com/a/111111/2195426))
```javascript
function sayHello2(name) {
  var text = 'Hello ' + name; // Local variable
  var say = function() { console.log(text); }
  return say;
}
var say2 = sayHello2('Bob');
say2();
// logs "Hello Bob"
```
- In JavaScript, you can think of a function reference variable as having both a pointer to a function as well as a hidden pointer to a closure. (two pointers)
- In JavaScript, if you use the function keyword inside another function, you are creating a closure. (function inside function => closure)
- In C and most other common languages, after a function returns, all the local variables are no longer accessible because the stack-frame is destroyed. But in JavaScript, if you declare a function within another function, then the local variables of the outer function can remain accessible after returning from it. The genius is that in JavaScript a function reference also has a secret reference to the closure it was created in.

## Remove duplicate values from an Array?
Use `Set` and `spreading`:
```javascript
const nums = [1,2,2,3,4,4,4,5];
const newNums = [...new Set(nums)];
console.log(newNums);
```
## How to check if an Object is an Array and Empty?
```javascript
if (arr && (Object.prototype.toString.call(arr) === '[object Array]' && arr.length === 0)) {
}
```
## How does the `this` keyword work? ([stackoverflow](https://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work))

### ThisBinding
 If a function is called on an object, such as in `obj.myMethod()` or the equivalent `obj["myMethod"]()`, then ThisBinding is set to the object (obj in the example). 
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