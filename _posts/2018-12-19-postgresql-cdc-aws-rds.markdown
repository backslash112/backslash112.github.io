---
layout: post
title:  "CDC (Change Data Capture) in PostgreSQL at AWS RDS"
date:  2018-12-19 20:01
description: "Change Data Capture is a way to track the change of the data from the database."
img: pg.png
tags: [AWS, PostgreSQL, db]
---

## What is Change Data Capture?
In databases, Change Data Capture is a way to track the change of the data from the database. Any change made to the records including INSERT, UPDATE and DELETE statements, will trigger the change event, the consumer can get the event name and the new data.
In PostgreSQL, we can enable the CDC via logical replication.

##  CDC in Amazon AWS RDS PostgreSQL
Beginning with PostgreSQL version 9.4, PostgreSQL supports the streaming of WAL changes using logical replication decoding. Amazon RDS supports logical replication for PostgreSQL version 9.4.9 and higher and 9.5.4 and higher.

### Step 1: Create a new parameter group with
```
rds.logical_replication = 1
wal_sender_timeout = 0
statement_timeout = 0
```

### Step 2: Create the logical replication slot
Connect to the database instance and run the following SQL statement
```sql
SELECT * FROM pg_create_logical_replication_slot('test_slot', 'test_decoding');
```

### Step 3: Enable logs for tables
By default, when you create a data table by CREATE TABLE statement, the table will enable the log.
If you don't want to enable the logs by default, you can use CREATE UNLOGGED TABLE statement.
If you want to disable the logs for a specific table, you can run the following statement:
```sql
ALTER TABLE <table name> SET UNLOGGED;
```
To enable the logs, run
```sql
ALTER TABLE <table name> SET LOGGED;
```

### Step 4: Listen to the change of data
In Node.js, use https://www.npmjs.com/package/pg-logical-replication


## Important: PostgreSQL WAL space out of control (Update: Jan 8, 2019)
Please make sure to delete the slot when you don't need it anymore. If not, you will get a high disk alarm or STORAGE_FULL error.

The reason is when you create a slot, PostgreSQL will records the WAL logs and keep all of them. And since there is no one consume the logs, then the WAL logs will getting larger and larger.

## References
  - [PostgreSQL WAL space out of control? Check your slots...](https://www.couyon.net/blog/postgresql-wal-space-out-of-control-check-your-slots)
