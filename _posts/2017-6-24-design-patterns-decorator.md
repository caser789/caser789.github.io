---
layout: post
title: Design patterns decorator
date:   2017-06-24 21:18:01 +0800
categories: pattern design decorator
tag: pattern
---

* content
{:toc}


## 目标

1. 动态的给对象增加方法或者属性


## 针对的问题

1. 给对象增加属性或者方法
2. 不能通过继承，因为继承是静态的，并且作用于整个类

## 其他


windows 增加 boarder, Vertical SB, Horizontal SB,
使用继承需要复杂的多继承，而使用装饰器可以一层层装饰，像 pipeline

## check list

1. 一个核心功能，多个可以组合的功能
2. 构建最小公约数接口
3. 

## 实现

```
"""
Attach additional responsibilities to an object dynamically.
Decorators privide a flexible alternative to subclassing for extending functionality.
"""

import abc


class Component(metaclass=abc.ABCMeta):
    """
    Define the interface for objects that can have responsibilities added to
    them dynamically
    """

    @abc.abstractmethod
    def operation(self):
        pass


class Decorator(Component, metaclass=abc.ABCMeta):
    """
    Maintain a referrence to a Component object and define an interface that 
    comforms to Components' interface
    """

    def __init__(self, component):
        self._component = component

    @abc.abstractmethod
    def operation(self):
        pass


class ConcreteDecoratorA(Decorator):
    """
    Add responsibilities to the component
    """
    
    def operation(self):
        # ...
        self._component.operation()
        # ...


class ConcreteDecoratorB(Decorator):
    """
    Add responsibilities to the component
    """
    
    def operation(self):
        # ...
        self._component.operation()
        # ...


class ConcreteComponent(Component):
    """
    Define an object to which additional responsibilities can be added
    """

    def opration(self):
        pass


def main():
    concrete_component = ConcreteComponent()
    concrete_decorator_a = ConcreteDecoratorA(concrete_component)
    concrete_decorator_b = ConcreteDecoratorB(concrete_component)
    concrete_decorator_b.operation()


if __name__ == '__main__':
    main()
```

## example

```
import abc


class Interface(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def refresh(self):
        pass


class Window(Interface):

    def refresh(self):
        print 'show dindow'


class Component(Interface):

    __metaclass__ = abc.ABCMeta

    def __init__(self, window):
        self._window = window

    @abc.abstractmethod
    def refresh(self):
        pass


class Border(Component):

    def refresh(self):
        print 'show border'
        self._window.refresh()


class SB(Component):

    def refresh(self):
        print 'show scroll bar'
        self._window.refresh()


def main():
    window = Window()
    window = Border(window)
    window = SB(window)
    window.refresh()


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
