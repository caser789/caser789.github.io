---
layout: post
title: IAM Codecommit
date:   2017-05-14 08:02:01 +0800
categories: dynamodb aws codecommit iam
tag: iam
---

* content
{:toc}

# Codecommit policies
------------------------

## All 1
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1492074523000",
            "Effect": "Allow",
            "Action": [
                "codecommit:BatchGetRepositories",
                "codecommit:CreateBranch",
                "codecommit:GetBlob",
                "codecommit:GetBranch",
                "codecommit:GetObjectIdentifier",
                "codecommit:GetRepository",
                "codecommit:GetTree",
                "codecommit:GitPull",
                "codecommit:GitPush",
                "codecommit:ListBranches",
                "codecommit:UpdateDefaultBranch",
                "codecommit:UpdateRepositoryDescription"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}

```

## All 2
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1492074523000",
            "Effect": "Allow",
            "Action": [
                "codecommit:*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
