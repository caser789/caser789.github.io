---
layout: post
title: IAM DynamoDB to Kinesis
date:   2017-05-13 12:54:01 +0800
categories: dynamodb aws stream kinesis firehose
tag: iam
---

* content
{:toc}

# Import dynamodb stream to kinesis firehose
------------------------

## policy
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

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
