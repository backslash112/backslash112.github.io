---
layout: post
title:  "338. Counting Bits"
date:  2020-10-14
description: ""
img: algo.jpg
tags: [algo]
---

#### Problem

Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

Example 1:

```js
Input: 2
Output: [0,1,1]
```

Example 2:

```js
Input: 5
Output: [0,1,1,2,1,2]
```

Follow up:

- It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
- Space complexity should be O(n).
- Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.


Follow up 中的 *Space complexity should be **O(n)*** 可以看成一个提示：用一个 size 为 n 的 Arryaey。
Follow up 中的 *Can you do it in linear time O(n) / possibly in a single pass*? 可以看成另一个提示：用一个 for loop。

#### 思路
For every num i between 1...num, a num i 和 i+1的bit关系如下：如果在 bit 后面补一个 0，例如 bit 11 代表 integer 3，bit 110 代表 6，即bit 后补一个0后，对应的int值会double；如果后面补一个1，对应的int值会double+1;

```js
[0, 1, 1, 2, 1, 2]
 0   1  2  3  4  5
```

#### 根据以上思路写出的代码

```js
var countBits = function(num) {
  if (num == 0) return [0];
  const dp = Array(num+1).fill(0);
  dp[0] = 0;
  dp[1] = 1;
  for (let i = 2; i <= num; i++) {
    if (i % 2 == 0) {
      dp[i] = dp[i/2];
    } else {
      dp[i] = dp[(i-1)/2] + 1;
    }
  }
  return dp;
};
```

#### 可以提升的点

##### 第一个点

```js
if (i % 2 == 0) ...
dp[i] = dp[i/2]
else
dp[i] = dp[(i-1)/2] + 1
```

`dp[i] = dp[i/2] or dp[(i-1)/2]` 可以合并为一句，需要利用的知识点是 **Logical Right Shifts**：
For positive numbers, a single logical right shift divides a number by 2, throwing out any remainders. Example:

```
0101 is 5
0101 >> 1 will become 0010 (integer 2).
```

##### 第二个点

```js
dp[i] = dp[i >> 1] + 0
dp[i] = dp[i >> 1] + 1
```

也可以合并为一句：需要利用的知识点是：`&`。因为代码中 `i` 是奇数位会加 1，偶数位不需要加 1，则可以用 `i & 1`。原理如下：

`2 & 1 = 0`
因为 2 的 bit 值是 `10`，`10 & 1` => `10 & 01 = 00`
`3 & 1 = 1`
因为 3 的 bit 值是 `11`，`11 & 1` => `11 & 01 = 01`
`99 & 1 = 1`
因为 99 的 bit 值是 `1100011`，`1100011 & 1` => `1100011 & 0000001 = 0000001`
所以，`n & 1` 只需要关注 n 的 bit 值的最后一位是 1 还是 0，即 n 是奇数还是偶数。
