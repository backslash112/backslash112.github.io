---
layout: post
title:  "MongoDB Aggregate Join"
date:  2020-03-26
description: ""
img: db.png
tags: [db]
---

## simple join two collections

```js
db.delivery.aggregate([
  {
    $lookup: {
      localField: 'deliveryId',
      from: 'item',
      foreignField: 'deliveryId',
      as: 'items'
    }
  }
])
```

## $expr

Support to compare two fields.


https://docs.mongodb.com/manual/reference/operator/query/expr/#op._S_expr

```js
{ "_id" : 1, "category" : "food", "budget": 400, "spent": 450 }
{ "_id" : 2, "category" : "drinks", "budget": 100, "spent": 150 }
{ "_id" : 3, "category" : "clothes", "budget": 100, "spent": 50 }
{ "_id" : 4, "category" : "misc", "budget": 500, "spent": 300 }
{ "_id" : 5, "category" : "travel", "budget": 200, "spent": 650 }
```

The following operation uses $expr to find documents where the spent amount exceeds the budget:

```js
db.monthlyBudget.find( { $expr: { $gt: [ "$spent" , "$budget" ] } } )
```

The operation returns the following results:

```js
{ "_id" : 1, "category" : "food", "budget" : 400, "spent" : 450 }
{ "_id" : 2, "category" : "drinks", "budget" : 100, "spent" : 150 }
{ "_id" : 5, "category" : "travel", "budget" : 200, "spent" : 650 }
```


https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/

Create a collection orders with the following documents:

```js
db.orders.insert([
  { "_id" : 1, "item" : "almonds", "price" : 12, "ordered" : 2 },
  { "_id" : 2, "item" : "pecans", "price" : 20, "ordered" : 1 },
  { "_id" : 3, "item" : "cookies", "price" : 10, "ordered" : 60 }
])
```

Create another collection warehouses with the following documents:

```js
db.warehouses.insert([
  { "_id" : 1, "stock_item" : "almonds", warehouse: "A", "instock" : 120 },
  { "_id" : 2, "stock_item" : "pecans", warehouse: "A", "instock" : 80 },
  { "_id" : 3, "stock_item" : "almonds", warehouse: "B", "instock" : 60 },
  { "_id" : 4, "stock_item" : "cookies", warehouse: "B", "instock" : 40 },
  { "_id" : 5, "stock_item" : "cookies", warehouse: "A", "instock" : 80 }
])
```

The following operation joins the orders collection with the warehouse collection by the item and whether the quantity in stock is sufficient to cover the ordered quantity:

```js
db.orders.aggregate([
   {
      $lookup:
         {
           from: "warehouses",
           let: { order_item: "$item", order_qty: "$ordered" },
           pipeline: [
              { $match:
                 { $expr:
                    { $and:
                       [
                         { $eq: [ "$stock_item",  "$$order_item" ] },
                         { $gte: [ "$instock", "$$order_qty" ] }
                       ]
                    }
                 }
              },
              { $project: { stock_item: 0, _id: 0 } }
           ],
           as: "stockdata"
         }
    }
])
```

The operation returns the following documents:

```js
{ "_id" : 1, "item" : "almonds", "price" : 12, "ordered" : 2,
   "stockdata" : [ { "warehouse" : "A", "instock" : 120 }, { "warehouse" : "B", "instock" : 60 } ] }
{ "_id" : 2, "item" : "pecans", "price" : 20, "ordered" : 1,
   "stockdata" : [ { "warehouse" : "A", "instock" : 80 } ] }
{ "_id" : 3, "item" : "cookies", "price" : 10, "ordered" : 60,
   "stockdata" : [ { "warehouse" : "A", "instock" : 80 } ] }
```

The operation would correspond to the following pseudo-SQL statement:

```sql
SELECT *, stockdata
FROM orders
WHERE stockdata IN (SELECT warehouse, instock
                    FROM warehouses
                    WHERE stock_item= orders.item
                    AND instock >= orders.ordered );
```

## like the simple join operation, but adding the query part

https://stackoverflow.com/questions/36459983/aggregation-filter-after-lookup

```js
// works
db.delivery.aggregate([
  {
    $lookup: {
      from: 'item',
      pipeline: [
        {
          $match: {
            $and:
              [
                { deliveryId: 1001 },
                { quantity: 10 },
              ]
          }
        }
      ],
      as: 'items'
    }
  }
]).pretty()

```

```js
db.delivery.aggregate([
  {
    $lookup: {
      from: 'item',
      pipeline: [
        {
          $match: {
            $and:
              [
                { deliveryId: 1001 },
                // { quantity: 10 },
                { $eq: ['$$quantity', 10] }
              ]
          }
        }
      ],
      as: 'items'
    }
  }
]).pretty()
// "errmsg" : "unknown top level operator: $eq",
```


```js

db.delivery.aggregate([
  {
    $match: {
      deliveryName: 'carl'
    }
  },
  {
    $lookup: {
      from: 'item',
      let: { delivery_deliveryId: '$deliveryId' },
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                { $lt: ['$quantity', '$inventory'] },
              ]
            }
          }
        }
      ],
      as: 'items'
    }
  }
]).pretty()

```


```js
// works
db.delivery.aggregate([
  {
    $match: {
      deliveryName: 'carl'
    }
  },
  {
    $lookup: {
      from: 'item',
      let: { delivery_deliveryId: '$deliveryId' },
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                { $eq: ['$deliveryId', '$$delivery_deliveryId'] },
                { $gt: ['$quantity', '$inventory'] },
              ]
            }
          }
        }
      ],
      as: 'items'
    }
  }
]).pretty()
 ```

```js
db.delivery.aggregate([
  {
    $match: {
      deliveryName: 'carl'
    }
  },
  {
    $lookup: {
      from: 'item',
      let: { delivery_deliveryId: '$deliveryId' },
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                { $lt: ['$quantity', '$inventory'] },
              ]
            }
          }
        }
      ],
      as: 'items'
    }
  }
]).pretty()
```

For example:
```sql
SELECT * 
  FROM deliveries a, items b
  WHERE a.id = b.deliveryId 
    AND (
      b.size > b.eci.inventory OR
      b.eci.damaged > 0 OR
      b.eci.outOfStore > 0
    )
```
Rewrite above SQL to MongoDB Aggregation:

```sh
# deliveries    a
# items b
db.deliveries.aggregate([
  {
    $match: {
      storeId: '1001'
    }
  },
  {
    $lookup: {
      from: 'items',
      let: { id: '$id' }, # a.'$id'
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                { $eq: ['$deliveryId', '$$id'] }, # b.'$deliveryId', let.'$$id'
                {
                  $or:
                    [
                      { $gt: ['$size', '$eci.inventory'] }, # b
                      { $gt: ['$eci.damaged', 0] }, # b
                      { $gt: ['$eci.outOfStore', 0] } # b
                    ]
                }
              ]
            }
          }
        }
      ],
      as: 'items' # will become a subarray of result record
    }
  },
  {
    $match: {
      $expr: {
        $gt: [{ $size: '$items' }, 0]
      }
    }
  }
]).pretty()

```