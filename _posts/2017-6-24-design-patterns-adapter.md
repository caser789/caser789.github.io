---
layout: post
title: Design patterns adapter
date:   2017-06-24 23:18:01 +0800
categories: pattern design adapter
tag: pattern
---

* content
{:toc}


## 目标

1. 适配不同的接口


## 其他


## 实现

```
"""
Convert the interface of a class into another interface clients expect.
Adapter lets classes work together that couldn't otherwise because of imcompatible interfaces
"""
class Target(metaclass=abc.ABCMeta):

    """
    Define the domain-specific interface that Client uses.
    """
    def __init__(self):
        self._adaptee = Adaptee()

    @abc.abstractmethod
    def request(self):
        pass


class Adapter(Target):
    """
    Adapt the interface of Adaptee to the Target interface
    """
    def request(self):
        self._adaptee.specific_request()


class Adaptee(object):
    """
    Define an existing interface that needs adapting
    """
    def specific_request(self):
        pass


def main():
    adapter = Adapter()
    adapter.request()


if __name__ == '__main__':
    main()
```

## example

```
import abc


class Base(object):
    __metaclass__ = abc.ABCMeta

    def __init__(self):
        self._adaptee = Adaptee()

    @abc.abstractmethod
    def speak(self):
        pass


class Adapter(Base):

    def speak(self):
        self._adaptee.shuo()


class Adaptee(object):

    def shuo(self):
        print 'Hello'

def main():
    adapter = Adapter()
    adapter.speak()


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
