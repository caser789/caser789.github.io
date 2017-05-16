---
layout: post
title: Usage of Boto3
date:   2017-05-15 13:50:01 +0800
categories: dynamodb aws boto3 python
tag: python
---

* content
{:toc}

# Usage of boto3
------------------------

## operate dynamodb

```python
"""
name: update_service_devicetoken_mapping.py
author: John Xue
Update dynamodb table `service-devicetoken-mapping`
update element in mapping_value of item with hashkey like '*timezones' from type string to type int
"""
import boto3
from boto3.dynamodb.conditions import Attr
import sys
from dock.common.helpers import cached_property


class Table(object):

    def __init__(self, **kw):
        self.aws_access_key_id = kw.get('aws_access_key_id', '')
        self.aws_secret_access_key = kw.get('aws_secret_access_key', '')
        self.region_name = kw.get('region', 'us-east-1')

    def resource(self, service_name):
        return boto3.resource(service_name, **self.__dict__)

    @cached_property
    def dynamodb(self):
        return self.resource('dynamodb')

    @cached_property
    def mapping_table(self):
        return self.dynamodb.Table('service-devicetoken-mapping')

    def get_item(self):
        response = self.mapping_table.scan(FilterExpression=Attr('mapping_key').contains(':timezones'))
        for item in response['Items']:
            yield item


def get_config():
    if len(sys.argv) != 3:
        print 'Usage: python update_service_devicetoken_mapping.py <aws_access_key_id> <aws_secret_access_key>'
        return
    return dict(aws_access_key_id=sys.argv[1], aws_secret_access_key=sys.argv[2])

if __name__ == '__main__':
    config = get_config()
    table = Table(**config)
    for item in table.get_item():
        data_set = item[u'mapping_value']
        new_set = set(int(item) for item in data_set)
        table.mapping_table.update_item(
            Key={'mapping_key': item['mapping_key']},
            UpdateExpression='SET mapping_value = :val1',
            ExpressionAttributeValues={':val1': new_set}
        )
```

## refs

* [tutorial](http://boto3.readthedocs.io/en/latest/guide/dynamodb.html#updating-item)
* [source code](https://github.com/boto/boto3/blob/develop/boto3/dynamodb/conditions.py)
* [boto3](https://github.com/boto/boto3/blob/develop/boto3/session.py)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
