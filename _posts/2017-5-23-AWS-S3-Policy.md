---
layout: post
title: AWS Sample S3 Bucket Policy
date:   2017-05-24 04:11:01 +0100
categories: aws s3 policy
tag: s3
---

* content
{:toc}


## static

```json
{
    "Version": "2012-10-17",
    "Id": "Policy1469676737443",
    "Statement": [
        {
            "Sid": "Stmt1469676713823",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::static-appcloudbox/prefix1*"
        }
    ]
}

```

## china virtualspace

```json
{
    "Version": "2012-10-17",
    "Id": "Policy1469676737443",
    "Statement": [
        {
            "Sid": "Stmt1469676713823",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws-cn:s3:::virtualspace/app/*"
        }
    ]
}

```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
