---
layout: post
title: AWS Limitations
date:   2017-06-01 08:53:01 +0100
categories: aws limit
tag: aws
---

* content
{:toc}

## SQS Message

* format: XML, JSON, unformatted text
* special char: #x9 | #xA | #xD | #x20 to #xD7FF | #xE000 to #xFFFD | #x10000 to #x10FFFF
* message attributes: max = 10
* message size: 1 Kb ~ 256 Kb
* message batch: 10
* message retention: default 4 days, min 1 min, max 14 days
* message visibility timeout: 12 hours

## Dynamodb

* 256 tables per region
* 5 local secondary index
* 5 global secondary index
* partition key length: 1 byte ~ 2048 bytes
* sort key length: 1 byte ~ 1024 bytes
* item size:  400Kb

# refs

[SQS Message](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/limits-messages.html)
[DynamoDB](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
