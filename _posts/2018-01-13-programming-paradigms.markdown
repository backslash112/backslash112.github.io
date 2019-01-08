---
layout: post
title:  "Programming Paradigms"
date:   2018-01-13 02:02:22
description: "There are three main programming paradigms: Imperative programming, Declarative programming and Functional programming."
img: programming-paradigms.jpg
tags: [programming]
---

## There are three main programming paradigms: Imperative programming, Declarative programming and Functional programming.

## Imperative Programming
The main idea of imperative programming is focusing on the steps of execution.
For example you want to find out the numbers which bigger that number 5 from a collection which named `collection`, so with imperative programming idea you need to do this:
Step 1, create a variable to store the results which can be named `results`.
Step 2, iterate the collection.
Step 3, compare each number to target number 5, if the current number is bigger that 5 then add it to the variable `results`.
step 4, return the variable `results`.

And the code of the steps above is:
```java
List<int> results = new List<int>();
foreach(var num in collection)
{
    if (num > 5)
          results.Add(num);
}
```
From this example you can see, this kind of programming paradigm is very common, no matter which language you are using, like `C`, `C++`, `C#`, `Java`, `Javascript`, `BASIC`, `Python` or `Ruby`, you can write code with those languages in this way.


## Declarative Programming
Declarative programming is focusing on the data structure, with the data structure to express the logic of the program, the idea is different from the imperative programming, instead of focusing on the steps, "how to do", declarative programming focusing on "what you want".
For example SQL statement is a kind of declarative programming, here is a statement will do a similar thing as above example, but in a totally different way:
```sql
 SELECT * FROM collection WHERE num > 5
```

## Functional Programming
Functional programming is related with the declarative programming, because they all have the same idea: focus on what you want not how to do, it is a form of declarative programming but it isn't the only kind of declarative programming.
One key feature of functional programming is "first class values", that means that not only can you write functions, but you can write functions that write functions, you can pass functions as parameters to other functions and return them as values.
Most programming languages already support this paradigm, for example JavaScript, C# and Java.
Here is an example to write a functional code in Java:
```java
List<Number> results = collection.stream()
                                 .filter(n -> n > 5)
                                 .collect(Collectors.toList());
```
You can see that it's more clear and easy to understand in this way.
