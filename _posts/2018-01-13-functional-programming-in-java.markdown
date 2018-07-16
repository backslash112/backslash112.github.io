---
layout: post
title:  "Functional Programming in Java"
date: 2018-01-13 12:13:00
description: "The big change in Java SE 8 is the best-known new feature: Lambda expression, it takes an effort to bring Java into the world of functional programming. It provide a clear and concise way to write the code."
img: functional-javase8.png
tags: [programming]
---

The big change in Java SE 8 is the best-known new feature: Lambda expression, it takes an effort to bring Java into the world of functional programming.
It provide a clear and concise way to write the code.
OK, let's start with something simple to learn to how use it.


### A quick look at the Lambda expression
First, let define a very simple lambda expression:
```
x -> x + 1
```
This expression takes one argument named x, and uses the expression form the return x+1. Through this example we can easily know that a lambda expression is composed of three parts:
```
(Argument list) -> { Body }
```
Argument list, Arrow token and body.

The body can either a single expression or a statement block.
The bracketed in the argument list part can be omit if there is only one argument.
The statement block in the body part is the same as in a normal method, but if there is only one single expression, the body is simply evaluated and returned, and the curly braces also can be omit.
For example:
```
(int x, int y) -> { return x + y; }
(int x, int y) -> x + y
x -> x + 1
() -> 3.14
(String s) -> { System.out.println(s); }
```
The first expression takes two integer arguments named x and y, and uses the expression form to return x+y.
The second expression is as same as the first one, just omitted the curly braces and the `return` key word.
The third expression takes one argument and uses the expression form to return x+1.
The fourth expression takes no argument and simply return an number 3.14.
The fifth expression takes one String argument named s, and uses the block form to print the string to the console, and returns nothing.


About the third expression, I guess you probably want to ask two questions:
What's the type of the argument `x`?
What's the type of the whole expression?

Now let introduce another concept which is java.util.function package. The definition about it from the official document is "Functional interfaces provide target types for lambda expressions and method references.". So that's mean it's a class for the functional objects, how to store the functional objects.
Well, The answer is that it depends. In Java the same lambda expression could be bound to variables of different types.
OK, let's go back to our example of lambda expression: `x -> x + 1`, we can write two different Function types for the same expression:
```
Function<Integer, Integer> add = x -> x + 1;
Function<String, String> concat  = x -> x + 1;
```
The type of x in the first expression is Integer, it takes a integer argument which named `x` and use the expression form to return x+1.
The type of x in the second expression is String, it takes a string argument and use the expression from to return the result of concatenates the integer 1 to any string x.


### The type with two arguments
Now we know how to define the type with one argument, how to write the code if the expression takes two arguments? Let's say we want to summarize two integer arguments x and y then return the result?
Well, we need another class to do that, the BiFunction<T, U, R> type, the T is mean the type of the first argument, the U mean the type of the second argument, and the R mean the type of the return value. For example:
```
BiFunction<Integer, Integer, Integer> sum = (x, y) -> x + y;
```


### The type with arguments but return value
How to define a lambda expression if it doesn't need to return a value?
The answer is use the `Consumer<T>` class, like this:

```
Consumer<String> sayHi = name -> System.out.println("hi, " + name);
```
P.S.: If you need two arguments you need to use 'BiConsumer<T, U>' class.


### The type without any argument
If we want to define a expression without any argument but you want to return a value, you need to use the `Supplier<T>` class, here is an example for it:
```java
Supplier<String> getName = () -> "Carl";
```
This lambda expression will return a String as result when you call it each time.


### How to invoke the lambda expression?
After we defined our lambda expressions, we need to use the `apply()` method to invoke these functions:
```
Integer result = add.apply(2);  // yields 3
String answer = concat.apply("hi");  // yields "hi1"
Integer  total = sum.apply(1, 2);  // yields 3
String name = getName.apply();  // yields "Carl"
```

1. [*Beginning Java objects* by Jacquie Barker](https://www.amazon.com/Beginning-Java-Objects-Concepts-Second/dp/1590594576)
2. [Java SE 8: Lambda Quick Start](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html)
3. [The big change in Java SE 8](https://www.javacodegeeks.com/2014/07/java-se-8-new-features-tour-functional-programming-with-lambda-expression.html)
4. [Functional Programming with Java 8 Functions](https://dzone.com/articles/functional-programming-java-8)
