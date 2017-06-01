---
layout: post
title: DynamoDB Concepts
date:   2017-05-23 04:47:01 +0100
categories: dynamodb aws hashkey rangekey
tag: dynamodb
---

* content
{:toc}

## 1. What is redynadb

RedynaDB 是一个封装了AWS DynamoDB 基本操作的 python module, 它使用 dynamoDB 作持久存储, 也可以选择使用高可用的 Redis 作为 cache。

## 2. Core Concepts

### NoSQL

NoSQL 即 not only SQL，其产生的背景是随着访问量以及随之而来的数据量大大增加的情况下，使用传统的 RDBMS
就需要人为地添加机器或者 shard 数据到不同的机器，这样的工作繁琐而困难。DynamoDB 采用了 consistent hashing
这样的方法，使得 sharding, replica 对使用者基本上是透明的，大大降低了运维的成本。

### table

就像其他 RDBMS，DynamoDB 将数据储存在 table 中，table 也相当于 mongoDB 的 collection。一张 table 就是一组数据的集合。

### item

每个 table 由多条 item 组成，而一个 item 由一组 attribute 组成。DynamoDB 中的 item 就像 RDBMS 的 row, record, tuple，相当于 mongoDB 的 document。

### attribute

每个 item 由一个或多个 attribute 组成。一个 attribute 是储存数据的基本单位，类似其他 RDBMS 中的 filed, column。类似 mongoDB 中 document 中的 key-value 对。
每张表是 schemaless，意味着无论是每个 item 包含哪些 attribute 还是每个 attribute 的数据是什么类型都不需要提前定义。每个 item 可以拥有自己独特的 attribute。但是每个 item 必须有一个 attribute 作为 hash key，用于 consistent hashing，还有一个可选的 range key，用于排序。
如果 range key 存在，则 hash key 可以重复，hash key + range key 唯一确定一个 item 即可。如果 range key 不存在，则 hash key 必须唯一。


### primary key

DynamoDB 支持2种主键：单独一个 hash key 的简单主键，以及由 hashkey 和 rangekey 组成的复合主键。

### hash key

hash key 也叫 Partition key, DynamoDB 使用 hash key 的 value 作为其分区 hash function 的输入，function 的值决定了 item
储存在 DynamoDB 的物理分区。相同的 hash key 只会存在同一 partition 上，因此相同 hash key，不同 range key 的 item 会存在同一分区。
如果 hash key 没有选择好，会产生过热的分区，影响效率。

### rangekey

在有 hashkey 的前提下可以选 item 中的一个 attribute 作为 range key。range key 也叫 sort key，DynamoDB 把相同 hahskey 的 item 存在同一个分区，以 sort key 的 value 进行排序

### secondary index

可以把 primary key 看作是 primary index, 其他的 index 都是 secondary index。secondary index 分为 local secondary index
和 global secondary index 两种。你如果想使用 hash key, range key 以外的 attribute 进行查询，则必须为此 table 建立
secondary index。如果 hash key 与 primay key 相同，则选择 local secondary index，否则应该建立 global secondary
index。Global secondary index 实质上是另外一张 table, 只不过与主 table 保持 eventually consistent。

## 3. 数据类型


## 4. 常见操作

* put_item
* update_item
* delete_item
* get_item
* query_table

## 5. 收费模式

## 6. Best Practice

* With cache
* Without cache


# refs
* [Key Concept](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
* [Data Type](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html)


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
