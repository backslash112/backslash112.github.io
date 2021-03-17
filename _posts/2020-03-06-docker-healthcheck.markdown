---
layout: post
title:  "Heath check in Docker"
date:  2020-03-06
description: ""
img: js.jpg
tags: [js]
---

## Create an array containing 1...N

```js
const n = 10
let a = Array.from(Array(n).keys());
console.log(a);
// [0,1,2,3,4,5,6,7,8,9]
// or shorter version using spread operator
a = [...Array(10).keys()];
console.log(a);
// [0,1,2,3,4,5,6,7,8,9]
```

## Refrences

[Programming Languages, Part A](https://www.coursera.org/learn/programming-languages/home/welcome)