---
layout: post
title: Design patterns facade
date:   2017-06-26 03:15:01 +0800
categories: pattern design facade
tag: pattern
---

* content
{:toc}


## 目标

1. 为子系统提供同一的接口

## 与其他模式关系

1. Facade 定义新接口，Adaptor 使用旧接口
2. Facade 通常是 singleton


## 实现

```
"""
Provide a unified interface to a set of interfaces in a subsystem.
Facade defines a higher-level interface that makes the subsystem easier to use.
"""


class Facade:
    """
    Know which subsystem classes are responsible for a request.
    Delegate client requests to appropriate subsystem objects.
    """
    def __init__(self):
        self._subsystem_1 = Subsystem1()
        self._subsystem_2 = Subsystem2()

    def operation(self):
        self._subsystem_1.operation1()
        self._subsystem_1.operation2()
        self._subsystem_2.operation1()
        self._subsystem_2.operation2()


class Subsystem1:
    """
    Implement subsystem functionality.
    Handle work assigned by the Facade object.
    Have no knowledge of the facade; that is, they keep no referrence to it.
    """

    def operation1(self):
        pass

    def operation2(self):
        pass


class Subsystem2:
    """
    Implement subsystem functionality.
    Handle work assigned by the Facade object.
    Have no knowledge of the facade; that is, they keep no referrence to it.
    """

    def operation1(self):
        pass

    def operation2(self):
        pass


def main():
    facade = Facade()
    facade.operation()

if __name__ == '__main__':
    main()
```

## example
```
class Stock(object):
    def buy(self):
        print 'buy stock'

    def sell(self):
        print 'sell stock'


class Bond(object):
    def get(self):
        print 'get bond'

    def set(self):
        print 'set bond'


class Debt(object):
    def load(self):
        print 'load debt'

    def dump(self):
        print 'dump debt'


class Fund():

    def __init__(self):
        self.stock = Stock()
        self.bond = Bond()
        self.debt = Debt()

    def buy(self):
        self.stock.buy()
        self.bond.set()
        self.debt.load()

    def sell(self):
        self.stock.sell()
        self.bond.get()
        self.debt.dump()


def main():
    fund = Fund()
    fund.buy()
    fund.sell()

if __name__ == "__main__":
    main()
```


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
