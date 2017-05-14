---
layout: post
title: IAM Quickinsight
date:   2017-05-14 09:52:01 +0800
categories: dynamodb aws iam quickinsight
tag: iam
---

* content
{:toc}

# Quickinsight policies
------------------------

## All
```json
{
       "Statement": [
          {
             "Action": [
                "iam:ListPolicyVersions",
                "iam:ListAccountAliases",
                "iam:AttachRolePolicy",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:CreateRole",
                "iam:CreatePolicy",
                "iam:CreatePolicyVersion",
                "iam:DeletePolicyVersion",
                "iam:GetRole",
                "iam:ListAttachedRolePolicies",
                "iam:ListRoles",
                "ds:CheckAlias",
                "ds:CreateIdentityPoolDirectory",
                "ds:CreateAlias",
                "ds:AuthorizeApplication",
                "ds:DescribeDirectories",
                "ds:UnauthorizeApplication",
                "ds:DeleteDirectory",
                "quicksight:Subscribe",
                "quicksight:Unsubscribe",
                "s3:ListAllMyBuckets"
             ],
             "Effect": "Allow",
             "Resource": [
                "*"
             ]
          }
       ],
       "Version": "2012-10-17"
}
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
