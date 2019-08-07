---
layout: post
title:  "Learn AWS DynamoDB with Node.js by Examples"
date:  2019-08-07
description: ""
img: ddb.png
tags: [js]
---

### Example 1: Insert One Record
```js
const params = {
  TableName: "store",
  Item: {
    "storeId": 1001,
    "storeName": "7-11",
    "updateTime": Date.now()
  }
};
client.put(params, (err, data) => {
  if (err) {
    console.log(err);
    return resolve();
  }
  console.log(data);
  return resolve();
});
```

### Example 2: Get One Record
```js
const params = {
  TableName: 'store',
  Key: {
    "storeId": 1001,
    "updateTime": 1565205241170
  }
};
client.get(params, (err, data) => {
  if (err) {
    console.log(err);
    return resolve(JSON.stringify(err.message, null, 2));
  }
  console.log(data.Item);
  resolve(data.Item);
})
```

### Example 3: Update One Record
```js
const params = {
  TableName: 'store1',
  Key: {
    "storeId": 1001
  },
  UpdateExpression: "set storeName = :n",
  ExpressionAttributeValues: {
    ':n': '7-eleven'
  },
  ReturnValues: "UPDATED_NEW"
};
client.update(params, (err, data) => {
  if (err) {
    console.log(err);
    return resolve(JSON.stringify(err.message, null, 2));
  }
  console.log(data.Item);
  resolve(data.Item);
});
```

### Example 4: Get All Records from One Particular Partition
```js
const params = {
  TableName: 'store1',
  KeyConditionExpression: 'storeId = :s',
  ExpressionAttributeValues: {
    ':s': 1001
  }
};
client.query(params, (err, data) => {
  if (err) {
    console.log(err);
    return resolve(JSON.stringify(err.message, null, 2));
  }
  console.log(data.Items);
  resolve(data.Items);
});
```

### Example 5: Get Records by Condition from All Records
```js
const params = {
  TableName: 'store1',
  FilterExpression: 'updateTime = :t',
  ExpressionAttributeValues: {
    ':t': 1565207433970
  }
};
client.scan(params, (err, data) => {
  if (err) {
    console.log(err);
    return resolve(JSON.stringify(err.message, null, 2));
  }
  console.log(data.Items);
  resolve(data.Items);
});
```

## Refrences
[Getting Started with DynamoDB SDK - Node.js and DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.NodeJs.html)