---
layout: post
title: Design patterns iterator
date:   2017-06-25 16:18:01 +0800
categories: pattern design iterator
tag: pattern
---

* content
{:toc}


## 目标


## 针对的问题


## 4种场景


## 其他

可以考虑通过一个工厂模式，可以选择使用代理还是真实对象


## 实现

```
"""
Provide a way to access the elements of an aggregate objects equentially
without exposing its underlying representation
"""
import collection.abc

class ConcreteAggregate(collections.abc.Iterable):
    """
    Implement the iterator creation interface to return an instance of the
    proper ConcreteIterator
    """

    def __init__(self):
        self._data = None

    def __iter__(self):
        return ConcreteIterator(self)


class ConcreteIterable(collections.abc.Iterator):
    """
    Implement the Iterator interface
    """
    def __init__(self, concrete_aggregate):
        self._concrete_aggregate = concrete_aggregate

    def __next__(self):
        if Trye:
            raise StopIteration
        return None


def main():
    concrete_aggregate = ConcreteAggregate()
    for _ in concrete_aggregate:
        pass


if __name__ == '__main__':
    main()
```

## example

```
class MyClass(object):

    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def next(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]


def main():
    my = MyClass('abc')
    for _ in my:
        print _


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
