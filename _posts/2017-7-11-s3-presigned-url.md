---
layout: post
title: S3 Presigned URL
date:   2017-07-11 02:01:01 +0800
categories: boto aws presigned s3
tag: s3
---

* content
{:toc}

## boto3

```python
import boto3
from botocore.client import Config

# Get the service client with sigv4 configured
s3 = boto3.client('s3', config=Config(signature_version='s3v4'))

# Generate the URL to get 'key-name' from 'bucket-name'
url = s3.generate_presigned_url(
    ClientMethod='get_object',
    Params={
        'Bucket': 'bucket-name',
        'Key': 'key-name'
    }
)
```

[ref](http://boto3.readthedocs.io/en/latest/reference/services/s3.html#S3.Client.generate_presigned_url)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
