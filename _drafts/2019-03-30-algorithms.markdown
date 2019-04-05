---
layout: post
title:  "Algorithms"
date:  2019-04-02
description: ""
img: js.jpg
tags: [js]
---

## How to detect a loop in a linked list? ([stackoverflow](https://stackoverflow.com/questions/2663115/how-to-detect-a-loop-in-a-linked-list))
- Floyd's cycle-finding algorithm ([stackoverflow](https://stackoverflow.com/a/2663147/2195426))
  - The idea is to have two references to the list and move them at different speeds. Move one forward by 1 node and the other by 2 nodes.
  - If the linked list has a loop they will definitely meet.
  - Else either of the two references(or their next) will become null.
  - (Fast and slow / Turtle and Rabbit)
```java
boolean hasLoop(Node first) {

    if(first == null) // list does not exist..so no loop either
        return false;
    Node slow, fast; // create two references.
    slow = fast = first; // make both refer to the start of the list
    while(true) {
        slow = slow.next;          // 1 hop
        if(fast.next != null)
            fast = fast.next.next; // 2 hops
        else
            return false;          // next node null => no loop
        if(slow == null || fast == null) // if either hits null..no loop
            return false;
        if(slow == fast) // if the two ever meet...we must have a loop
            return true;
    }
}
```
Here's a refinement of the Fast/Slow solution, which correctly handles odd length lists and improves clarity. ([stackoverflow](https://stackoverflow.com/a/3301796/2195426))

```java
boolean hasLoop(Node first) {
    Node slow = first;
    Node fast = first;

    while(fast != null && fast.next != null) {
        slow = slow.next;          // 1 hop
        fast = fast.next.next;     // 2 hops 

        if(slow == fast)  // fast caught up to slow, so there is a loop
            return true;
    }
    return false;  // fast reached null, so the list terminates
}
```

### Method 1: Use the build-in functions: split, reverse and join
```javascript
function reverse(str) {
  return str.split('').reverse().join('');
}
```
### Method 2: Use the for loop

```javascript
function reverse(str) {
  let res = '';
  for (let i = str.length - 1; i >= 0; i--) {
    res += str[i];
  }
  return res;
}
```