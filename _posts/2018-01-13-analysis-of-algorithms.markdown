---
layout: post
title: "Analysis of Algorithms"
date: 2018-01-13 02:02:22
description: "When we try to analysis an algorithm, we can follow those steps: The size of the input; How to calculate the run time of an algorithm; The Best Efficiency, the Worst Efficiency and the Average Efficiency."
img: analysis-algorithms.jpg
tags: [algorithms]
---

## Update 2019-04-05

### Big O
- If we wanted to find a number in the list, this would be `O(n)` since at most we would have to look through **the entire list** to find our number. 
-  The Big-O is still `O(n)` even though we might find our number the first try and run through the loop once because Big-O describes the **upper bound** for an algorithm. ([stackoverflow](https://stackoverflow.com/a/3368/2195426))

### What is tail recursion?
Compare those two functions:
```javascript
function recsum(x) {
    if (x===1) {
        return x;
    } else {
        return x + recsum(x-1);
    }
}
// output:
/*
recsum(5)
5 + recsum(4)
5 + (4 + recsum(3))
5 + (4 + (3 + recsum(2)))
5 + (4 + (3 + (2 + recsum(1))))
5 + (4 + (3 + (2 + 1)))
15
*/
```
```javascript
function tailrecsum(x, running_total=0) {
    if (x===0) {
        return running_total;
    } else {
        return tailrecsum(x-1, running_total+x);
    }
}
// output:
/*
tailrecsum(5, 0)
tailrecsum(4, 5)
tailrecsum(3, 9)
tailrecsum(2, 12)
tailrecsum(1, 14)
tailrecsum(0, 15)
15
*/
```
In the tail-recursive case, with each evaluation of the recursive call, the running_total is updated.
([stackoverflow](https://stackoverflow.com/a/37010/2195426))

### How to find time complexity of an algorithm?

You add up how many machine instructions it will execute as a function of the size of its input, and then simplify the expression to the largest (when `N` is very large) term and can include any simplifying constant factor.
For example, lets see how we simplify `2N + 2` machine instructions to describe this as just `O(N)`.

#### Why do we remove the two `2s` ?

We are interested in the performance of the algorithm as `N` becomes large.
Consider the two terms `2N` and `2`.
What is the relative influence of these two terms as N becomes large? Suppose `N` is a million.
Then the first term is 2 million and the second term is only `2`.
For this reason, we drop all but the largest terms for large `N`.
So, now we have gone from `2N + 2` to `2N`.
([stackoverflow](https://stackoverflow.com/a/11032063/2195426))

___
## What is analysis of algorithms?
The definition of the technical level about analysis of algorithms is:<br>
Study the use efficiency of **time** and **memory space** when an algorithm is running.<br>
That's mean focus on the time efficiency and space efficiency.<br>

Time efficiency means how fast when invoke an algorithm.<br>
Space efficiency means how many extra space will be used when invoke an algorithm.<br>

Early in the computer age, the time and the space, those two resources are both expensive. But now with the increase of the technology, the space efficiency is not an importance problem anymore. We only need to focus on the time efficiency.

**So that's mean when we analysis an algorithm, we only care the speed.**


## A common idea of analysis of algorithms:

When we try to analysis an algorithm, we can follow those steps:

### Step 1: the size of the input
Firstly, we need figure out what's the size of the input, the parameter, in other words, how big of the problem we trying to solve?<br>
Start with here because there is a general rule, no matter which algorithm you are using, larger size of the input will cost longer time to invoke.<br>
The size of the input depends on the particular problem, the same input can be different in different situations. For example an algorithm for spell checking.<br>
If you focus on each word, then the length of the input just the size of the input.<br>
But if you focus on the phrase, then the size of the input will becoming smaller.<br>


### Step 2: How to calculate the run time of an algorithm?
And the second step is to calculate the speed of an algorithm.<br>
You can simply using a stopwatch to find out how much time an algorithm spend while running.<br>
But there is a big problem is when you invoke the same algorithm on another computer, you will get a different result. It depends on the performance of the computer.<br>
So is there a better way to do that?<br>
The answer is YES.<br>
We can calculate **how many steps it invoked**, to make it more simple, we can only focus on the most important operation, which called "Basic Operation".


### Step 3: The Best Efficiency, the Worst Efficiency and the Average Efficiency
Sometimes, the efficiency of an algorithm depends on the details of the input.<br>
For example a simple searching algorithm, let's say to find the number 9 from the list sequentially.<br>
We have two lists, which are:
```
list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9]
list2 = [9, 1, 2, 3, 4, 5, 6, 7, 8]
```
It's easy to know that it will be quicker to find the number 9 from the list2.<br>
Generally, we call the efficiency of an algorithm **"The Best Efficiency"** when the input is as good as list2.<br>
And call it **"The Worst Efficiency"** when the input is as bad as list1.

In the actual condition, the input data will be random, neither "the best input" nor "the worst input".<br>
So in the actual condition, we call it **"The Average Efficiency"**.<br>

**Now how to figure out the average efficiency of an algorithm is our question.**<br>
First of all, you can't use the average value of the best efficiency and the worst efficiency, even sometimes the real average efficiency equals that value coincidentally.<br>
The correct method is we need to do some hypothesis about the input data `n`. Let's using the example above to explain it, there are two hypotheses:
1. No matter you choose witch number as your target number, you always can find it from the list successfully. Now the probability to find the target is p(0 <= p <= 1).
2. For any target number `i`, the probability of the target number located in position `i` are same.

From the two hypotheses above we can see that:
1. If it always can find the given target `i` from the input, the probability is `p/n` that the target just located in position `1` for the first time. In this case, the number of comparison is 1.
2. If it always can't find the given target from the input, the number of comparisons is `n`. In this case the probability is `(1-p)`.

The average efficiency C(n) = `p(n+1) / 2 + n(1-p)`
```
C(n) = [1 * p/n + 2 * p/n + ... + i * p/n + ... + n * p/n] + n*(1-p)
=  p/n[1 + 2 + ... + i + ... + n] + n(1-p)
= p/n * n(n+1)/2 + n(1-p)
= p(n+1) / 2 + n(1-p)               
```

We can see that,
1. If `p = 1`, that's mean the searching must be success, the result is `(n+1)/2` witch means it need to search about half of the input data.
2. If `p = 0`, that's mean the searching must be fail, the result is `n` witch means it need to search the whole input data.


## References：
[*Introduction to The Design and Analysis of Algorithms, Third Edition* by Anany Leitin](https://www.amazon.com/Introduction-Design-Analysis-Algorithms-3rd/dp/0132316811)
