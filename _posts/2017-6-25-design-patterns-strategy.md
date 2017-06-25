---
layout: post
title: Design patterns strategy
date:   2017-06-25 11:18:01 +0800
categories: pattern design strategy
tag: pattern
---

* content
{:toc}


## 目标

1. 定义一组算法，封装好每一个，使他们可以互换

## 针对的问题


## 与其他模式关系

1. 与 template 模式只是粒度不同

## 实现

```
"""
Define a family of algorithms, encapsulate each one, and make them 
interchangeable. Strategy lets the algorithm vary independently from clients
that use is
"""
import abc


class Context:
    """
    Define the interface of interest to clients.
    Maintain a referrence to a strategy object
    """
    def __init__(self, strategy):
        self._strategy = strategy

    def context_interface(self):
        self._strategy.algorithm_interface()

class Strategy(metaclass=abc.ABCMeta):
    """
    Declare an interface common to all supported algorithms.
    Context uses this interface to call algorithm defined by a ConcreteStrtegy
    """
    @abc.abstractmethod
    def algorithm_interface(self):
        pass

class ConcreteStrategyA(Strategy):
    """
    Implement the algorithm using the Strategy interface
    """
    def algorithm_interface(self):
        pass

class ConcreteStrategyB(Strategy):
    """
    Implement the algorithm using the Strategy interface
    """
    def algorithm_interface(self):
        pass

def main():
    concrete_strategy_a = ConcreteStrategyA()
    context = Context(concrete_strategy_a)
    context.context_interface()


if __name__ == '__main__':
    main()
```

## example

```
import abc


class Base(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def get_price(self):
        pass


class Normal(Base):

    def get_price(self, amount):
        return amount


class Threshold(Base):
    def __init__(self, threshold, minus):
        self.threshold = threshold
        self.minus = minus

    def get_price(self, amount):
        if amount > self.threshold:
            return amount - self.minus
        return amount


class Discount(Base):
    def __init__(self, percent):
        self.percent = percent

    def get_price(self, amount):
        return int(amount * self.percent)


class Store(object):

    __store__ = {
        'normal': Normal(),
        '300 minus 100': Threshold(300, 100),
        'discount 0.8': Discount(0.8)
    }

    def __init__(self, mode):
        self._strategy = self.__store__[mode]

    def checkout(self, amount):
        return self._strategy.get_price(amount)


def main():
    amount = 400
    store = Store('normal')
    print store.checkout(amount)
    store = Store('300 minus 100')
    print store.checkout(amount)
    store = Store('discount 0.8')
    print store.checkout(amount)


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
