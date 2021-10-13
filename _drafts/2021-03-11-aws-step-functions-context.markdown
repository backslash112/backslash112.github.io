---
layout: post
title:  "Use Context in Step Functions "
date:  2021-03-11
description: ""
img: js.jpg
tags: [js]
---

## AWS Step Functions

```json
{
  "Comment": "A Hello World example of the Amazon States Language using Pass states",
  "StartAt": "Hello",
  "States": {
    "Hello": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:xxx:function:test",
      "ResultPath": "$",
      "Next": "World",
      "Parameters": {
        "action": "BuildActivitySummaryPerDay",
        "storeIds.$": "$.context.storeId"
      }
    },
    "World": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:xxx:function:test",
      "ResultPath": "$",
      "Next": "done",
      "Parameters": {
        "context": {
          "storeId.$": "$$.Execution.Input.storeId"
        }
      }
    },
    "done": {
      "Type": "Pass",
      "Result": "World",
      "End": true
    }
  }
```
