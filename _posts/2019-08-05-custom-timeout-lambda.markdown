---
layout: post
title:  "Best practices in AWS: Custom Timeout"
date:  2019-08-03
description: ""
img: aws.png
tags: [js]
---

## Best practices with AWS

### The max execution time now is 15 mins, you should return ealier before the Lambda throwing the timeout error.

Here is a way to use a custom timeout:
```js
const { from, race } = require('rxjs');

module.exports.hello = async (event) => {

  function work() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('finished!');
      }, 4000);
    });
  }
  const workStream = from(work());

  function timeout() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('timeout!');
      }, 2000);
    });
  }
  const timeoutStream = from(timeout());

  const res = await new Promise((resolve, reject) => {
    race(workStream, timeoutStream)
      .subscribe(msg => {
        resolve(msg);
      })
  });

  return { res };
};

```