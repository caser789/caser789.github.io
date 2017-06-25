---
layout: post
title: Design patterns bridge
date:   2017-06-25 07:54:01 +0800
categories: pattern design bridge
tag: pattern
---

* content
{:toc}


## 目标

1. 解耦 abstraction 和 implementation, 解除它们之间的依赖
2. 接口和实现之间互相独立，不是继承的关系


## 针对的问题

1. 接口和实现都不能独立的扩展

## 其他

1. state 模式允许对象在不同的状态下拥有不同的方法


## check list

1. 功能是否存在正交关系

## 实现

```
"""
Decouple an abstraction from its implementation so that the two can vary independently
"""

import abc


class Abstraction(object):
    """
    Define the Abstraction's interface.
    Maintain a referrence to an object of type Implementor.
    """
    def __init__(self, imp):
        self._imp = imp

    def operation(self):
        self._imp.operation_imp()


class Implementor(metaclass=abc.ABCMeta):
    """
    Define the interface for implementation classes.
    This interface doesn't have to correspond exactly to Abstraction's interface;
    in fact the two interfaces can be quite different.
    Typically the implementor interface provices only primitive operations,
    and Abstraction define higher-level operations based on these primitives.
    """
    @abc.abstractmethod
    def operation_imp(self):
        pass


class ConcreteImplementorA(Implementor):
    """
    Implement the Implementor interface and define its concrete implementation.
    """
    def operation_imp(self):
        pass


class ConcreteImplementorB(Implementor):
    """
    Implement the Implementor interface and define its concrete implementation.
    """
    def operation_imp(self):
        pass


def main():
    concrete_implementor_a = ConcreteImplementorA()
    abstraction = Abstraction(concrete_implementor_a)
    abstraction.operation()


if __name__ == '__main__':
    main()
```

## example

```
import abc

class Phone(object):
    __metaclass__ = abc.ABCMeta

    def __init__(self):
        self._software = None

    def install_software(self, software):
        self._software = software

    @abc.abstractmethod
    def run(self):
        pass


class Iphone(Phone):
    def run(self):
        print 'Lanuch Iphone'
        if self._software:
            self._software.run_software()


class Samsang(Phone):
    def run(self):
        print 'Lanuch Samsung'
        if self._software:
            self._software.run_software()


class Software(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def run_software(self):
        pass


class FlappyBird(Software):
    def run_software(self):
        print 'bird flying'


class PlantVSZombies(Software):
    def run_software(self):
        print 'zombies running'

def main():
    iphone = Iphone()
    iphone.run()
    iphone.install_software(FlappyBird())
    iphone.run()
    iphone.install_software(PlantVSZombies())
    iphone.run()
    samsang = Samsang()
    samsang.run()
    samsang.install_software(FlappyBird())
    samsang.run()
    samsang.install_software(PlantVSZombies())
    samsang.run()


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
