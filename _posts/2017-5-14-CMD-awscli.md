---
layout: post
title: AWSCli Usage
date:   2017-05-14 10:14:01 +0800
categories: cmd awscli aws
tag: cmd
---

* content
{:toc}

# Config
------------------------

```
$ aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: us-east-1
Default output format [None]:
```

# S3
------------------------

## Upload

```
aws s3 cp {local_filename} s3://u5games/bzmst/{remote_filename}
```
## Download

```
aws s3 cp s3://u5games/bzmst/{remote_filename} {local_filename}
```
## List

```
aws s3 ls s3://u5games/bzmst --recursive --human-readable --summarize
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
