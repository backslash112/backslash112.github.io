---
layout: post
title:  "Three ways to write the filter function."
date:  2019-09-09
description: ""
img: js.jpg
tags: [js]
---

Here are two new ways I learned from coursera course: Programming Languages, Part A.
### filter1: use a loop
```js
// use loop
const filter1 = (f, list) => {
  if (!list) return list;
  const res = [];
  list.forEach(num => {
    if (f(num)) {
      res.push(num);
    }
  });
  return res;
}
```

### filter2: use recursion
```js
// use recursion
const filter2 = (f, list) => {
  if (list.length === 0) {
    return [];
  }
  const first = list[0];
  if (f(first)) {
    return [first, ...filter2(f, list.slice(1))];
  }
  return filter2(f, list.slice(1));
}
```

### filter3: use recursion with accumulator
```js
// use recursion with accumulator
const filter3 = (f, list, acc) => {
  if (list.length === 0) {
    return acc;
  }
  const first = list[0];
  if (f(first)) {
    acc.push(first);
    
  }
  return filter2(f, list.slice(1),acc);
}
const res1 = filter1(x => x > 0, [-1,0,1,2,3]);
const res2 = filter2(x => x > 0, [-1,0,1,2,3]);
const res3 = filter3(x => x > 0, [-1,0,1,2,3], []);
```
### result:
```js
console.log(res1);
console.log(res2);
console.log(res3);
// [1,2,3]
// [1,2,3]
// [1,2,3]
```

## Refrences
[Programming Languages, Part A](https://www.coursera.org/learn/programming-languages/home/welcome)