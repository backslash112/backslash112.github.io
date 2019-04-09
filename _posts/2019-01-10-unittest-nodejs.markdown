---
layout: post
title:  "Unit Testing with Node.js"
date:  2019-01-10
description: "As a software engineer, I am very agree with this rule: \"No tests, no code!\""
img: unit-test.png
tags: [unit-test, node.js]
---

## Unittest with MongoDB (In-memory Database)
Document for the idea of in-memory MongoDB instance: 
[https://jestjs.io/docs/en/mongodb](https://jestjs.io/docs/en/mongodb)

A working example:
[https://github.com/backslash112/jest-mongodb](https://github.com/backslash112/jest-mongodb)


## Useful Commands

### Use dotenv Envirement varible in Jest:
```sh
jest --setupFiles dotenv/config
```

### Show converage report in Jest:
```sh
jest --coverage
```

### Run all tests serially in the current process:
```sh
jest --runInBand
 ```

## Reference
- [https://jestjs.io/docs/en/cli](https://jestjs.io/docs/en/cli)
- [https://jestjs.io/docs/en/expect](https://jestjs.io/docs/en/expect)
