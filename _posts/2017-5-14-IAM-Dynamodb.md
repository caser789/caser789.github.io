---
layout: post
title: IAM DynamoDB Policies
date:   2017-05-14 09:46:01 +0800
categories: dynamodb aws stream kinesis firehose
tag: iam
---

* content
{:toc}

Dynamodb Policies
------------------------

## Import dynamodb stream to kinesis firehose
```json
{
    "Statement": [
        {
            "Action": [
                "logs:*"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Sid": "firehose"
        },
        {
            "Action": [
                "firehose:DescribeDeliveryStream",
                "firehose:ListDeliveryStreams",
                "firehose:PutRecord",
                "firehose:PutRecordBatch"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:firehose:us-east-1:115200798215:deliverystream/ohippo-muid"
            ],
            "Sid": "firehose2"
        },
        {
            "Action": [
                "dynamodb:DescribeStream",
                "dynamodb:DescribeTable",
                "dynamodb:GetItem",
                "dynamodb:GetRecords",
                "dynamodb:GetShardIterator",
                "dynamodb:ListStreams",
                "dynamodb:ListTables"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Sid": "dynamo"
        },
        {
            "Action": [
                "kinesis:DescribeStream",
                "kinesis:ListStreams",
                "kinesis:GetShardIterator",
                "kinesis:GetRecords",
                "kinesis:ListTagsForStream"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Sid": "kinesis"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        }
    ]
}
```
## Table all actions

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:BatchGetItem",
                "dynamodb:BatchWriteItem",
                "dynamodb:CreateTable",
                "dynamodb:DeleteItem",
                "dynamodb:DescribeTable",
                "dynamodb:GetItem",
                "dynamodb:ListTables",
                "dynamodb:PutItem",
                "dynamodb:Query",
                "dynamodb:UpdateItem",
                "dynamodb:UpdateTable",
                "dynamodb:Scan"
            ],
            "Resource": [
                "arn:aws:dynamodb:us-east-1::table/em*"
            ]
        }
    ]
}
```
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
