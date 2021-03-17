---
layout: post
title:  "MongoDB $lookup pipeline"
date:  2020-03-06
description: ""
img: mongo.jpeg
tags: [mongodb, aggragate]
---

## An example of $lookup pipeline

### Here is the data set we have.
Two collections, `items` and `inventories`, those two collections connected via `storeId + itemId`.

```sh
> db.items.find()
{ "storeId" : 12200, "itemId" : 1001, "itemName" : "apple" }
{ "storeId" : 12200, "itemId" : 1002, "itemName" : "Sony headphone" }
{ "storeId" : 12201, "itemId" : 1001, "itemName" : "apple2" }
{ "storeId" : 12201, "itemId" : 1002, "itemName" : "Sony headphone 2" }

> db.inventories.find()
{ "storeId" : 12200, "itemId" : 1002, "quantity" : 3 }
{ "storeId" : 12200, "itemId" : 1001, "quantity" : 2 }
{ "storeId" : 12201, "itemId" : 1002, "quantity" : 12 }
{ "storeId" : 12201, "itemId" : 1001, "quantity" : 11 }
```

### If we want to join those two collections and return the full fields, in SQL we will use `INNER JOIN` like this:

```sql
SELECT items.storeId, items.itemId, items.itemName, inventories.quantity 
FROM items
INNER JOIN inventories
ON items.storeId = inventories.storeId AND items.itemId = inventories.itemId

-- or

SELECT items.storeId, items.itemId, items.itemName, inventories.quantity
FROM items, inventories
WHERE items.storeId = inventories.storeId AND items.itemId = inventories.itemId
```

### In MongoDB, we have `$lookup` for table joining, and `$lookup` has two syntaxes:

#### Syntax #1

for single field matching.

```js
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

#### Syntax #2

For 2 and more fields matching.

```js
{
   $lookup:
     {
       from: <collection to join>,
       let: { <var_1>: <expression>, …, <var_n>: <expression> },
       pipeline: [ <pipeline to execute on the collection to join> ],
       as: <output array field>
     }
}
```

#### Here we should syntax #2 to convert above SQL statement into MongoDB aggregation

```js
db.items.aggregate([
  {
    "$lookup": {
      "from": "inventories",
      "let": {
        storeId: "$storeId",
        itemId: "$itemId"
      },
      "pipeline": [
        {
          "$match": {
            "$expr": {
              "$and": [
                { "$eq": ["$storeId", "$$storeId"]},
                { "$eq": ["$itemId", "$$itemId"]}
              ]
            }
          }
        }
      ],
      "as": "inventories_docs"
    }
  },
  {
    "$unwind": "$inventories_docs"
  },
  {
    "$addFields": {
      "quantity": "$inventories_docs.quantity"
    }
  },
  {
    "$project": {
      "inventories_docs": 0
    }
  },
  {
    "$match": { "itemId": { "$in": [1001,1002]}, "storeId": 12200 }
  }
]).pretty()
```

#### Note below version is WRONG which using syntax #1 to join two fields

```js
db.items.aggregate([
  {
    "$lookup": {
      "from": "inventories",
      "localField": "itemId",
      "foreignField": "itemId",
      "as": "inventories_docs"
    }
  },
  {
    "$unwind": "$inventories_docs"
  },
  {
    "$addFields": {
      "quantity": "$inventories_docs.quantity"
    }
  },
  {
    "$project": {
      "inventories_docs": 0
    }
  },
  {
    "$match": { "itemId": { "$in": [1001]}, "storeId": 12200 }
  }
]).pretty()
```
