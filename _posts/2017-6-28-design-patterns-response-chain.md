---
layout: post
title: Design patterns response chain
date:   2017-06-28 11:18:01 +0000
categories: pattern design chain response
tag: pattern
---

* content
{:toc}


## 目标

## 针对的问题
## 4种场景
## 其他
## 与其他模式关系
## 实现

```python
import abc


class Handler(object):
    __metaclass__ = abc.ABCMeta

    def __init__(self):
        self.successor = None

    def set_successor(self, successor):
        self.successor = successor

    @abc.abstractmethod
    def handle_request(self, request):
        pass


class ConcreteHandler(Handler):

    def handle_request(self, request):
        if 'condition met':
            'handle request'
        else:
            self.successor.handle_request(request)
```

## example

```python
import abc


class Handler(object):
    __metaclass__ = abc.ABCMeta

    def __init__(self, name):
        self.name = name
        self.successor = None

    def set_successor(self, successor):
        self.successor = successor

    @abc.abstractmethod
    def handle_request(self, request):
        pass


class Director(Handler):

    def handle_request(self, request):
        if request.amount < 5:
            print 'handle {request.purpose} at {request.amount} by {self.name}'.format(self=self, request=request)
        else:
            self.successor.handle_request(request)


class VicePresident(Handler):

    def handle_request(self, request):
        if request.amount < 10:
            print 'handle {request.purpose} at {request.amount} by {self.name}'.format(self=self, request=request)
        else:
            self.successor.handle_request(request)


class President(Handler):

    def handle_request(self, request):
        if request.amount < 50:
            print 'handle {request.purpose} at {request.amount} by {self.name}'.format(self=self, request=request)
        else:
            self.successor.handle_request(request)


class Congress(Handler):

    def handle_request(self, request):
        print 'handle {request.purpose} at {request.amount} by {self.name}'.format(self=self, request=request)


class PurchaseRequest(object):

    def __init__(self, amount, number, purpose):
        self.amount = amount
        self.number = number
        self.purpose = purpose


def main():
    wjzhang = Director('wuji.zhang')
    gy = VicePresident('guo.yang')
    jg = President('jing.guo')
    meeting = Congress('wulindahui')

    wjzhang.set_successor(gy)
    gy.set_successor(jg)
    jg.set_successor(meeting)

    wjzhang.handle_request(PurchaseRequest(3, 10004, 'buy a house'))
    wjzhang.handle_request(PurchaseRequest(8, 10004, 'buy 5th street'))
    wjzhang.handle_request(PurchaseRequest(13, 10004, 'buy Manhattan'))
    wjzhang.handle_request(PurchaseRequest(53, 10004, 'buy New York'))
    wjzhang.handle_request(PurchaseRequest(153, 10004, 'buy America'))

if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
