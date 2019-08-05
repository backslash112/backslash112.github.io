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

Here I use `race` operator to take the first emitting:

![img](https://miro.medium.com/max/150/1*rMlMQMO2oRecJ408HH4ngQ.gif)

The reason I'm using this is: let's say there is a Lambda function, let's call it `work()`, at some points it takes longer that 15 mins which out of our control. Instead of let AWS Lambda to throw the Timeout error, when the `work()` process start running, I let another process (called `timeout()` at here, with config timeout 14 mins) to start together with `work()` function, then the `timeout()` will return at the point 14 mins which can allow us to catch this custom timeout outside.


## Refrences
[learn-rxjs: race](https://www.learnrxjs.io/operators/combination/race.html)

[Learn to combine RxJs sequences with super intuitive interactive diagrams](https://blog.angularindepth.com/learn-to-combine-rxjs-sequences-with-super-intuitive-interactive-diagrams-20fce8e6511)
