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

* 封装了AWS DynamoDB table 的基本操作的 python module
* dynamodb 作持久存储
* 高可用的 Redis 作为 cache (optional)

## 2. Core Concepts

### table

一张 table 是一组 items 的集合

### item

item 是 dynamodb 储存数据的基本单位，类似 python 中的 dict 数据结构

### attribute

item 中的 key 叫做 attribute

### primary key

DynamoDB supports two different kinds of primary keys:

1. hashkey
2. hashkey + rangekey

### hashkey

* 也叫 Partition key
* 选择 item 中的一个 attribute 作为 hashkey
* DynamoDB 使用主键的 value 作为其分区 hash function 的输入
* hashkey 的 value 决定了 item 储存在 DynamoDB 的物理分区
* 在主键为 hashkey 的 table 中，不允许2个 items 的 hashkey 的 value 相同

### rangekey

* 也叫 sort key
* 在有 hashkey 的前提下选 item 中的一个 attribute 作为 rangekey
* DynamoDB 把相同 hahskey 的 item 存在同一个分区，以 sort key 的 value 进行排序

### secondary index

* primary key 为 primary index, 其他的 index 都是 secondary index
* Global secondary index – an index with a partition key and sort key that can be different from those on the table
* Local secondary index – an index that has the same partition key as the table, but a different sort key
* You can define up to 5 global secondary indexes and 5 local secondary indexes per table.

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
