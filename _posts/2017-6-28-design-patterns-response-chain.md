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
1. 一个入口，多个 handler
2. client launch and leave
## 针对的问题
## 其他
## 与其他模式关系
1. Chain of Responsibility, Command, Mediator, Observer 都是为了分离 sender from receiver
2. 可以用 Command 代表 request object
3. 与 composite 联用，a component's parent be its successor
## 实现

```python
import abc


class Handler(object):
    """
    Define an interface for handling requests.
    Implement the successor link.
    """
    __metaclass__ = abc.ABCMeta

    def __init__(self, successor=None):
        self._successor = successor

    @abc.abstractmethod
    def handle_request(self, request):
        pass


class ConcreteHandler1(Handler):
    """
    Handle request, otherwise forward it to the successor.
    """

    def handle_request(self, request):
        if 'condition met':
            'handle request'
        elif self._successor is not None:
            self._successor.handle_request(request)


class ConcreteHandler2(Handler):
    """
    Handle request, otherwise forward it to the successor.
    """

    def handle_request(self, request):
        if 'condition met':
            'handle request'
        elif self._successor is not None:
            self._successor.handle_request(request)

def main():
    concrete_handler_1 = ConcreteHandler1()
    concrete_handler_2 = ConcreteHandler2(concrete_handler_1)
    concrete_handler_2.handle_request('request')

if __name__ == '__main__':
    main()
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
