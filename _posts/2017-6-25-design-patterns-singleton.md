---
layout: post
title: Design patterns singleton
date:   2017-06-25 17:18:01 +0800
categories: pattern design singleton
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

```
"""
Ensure a class only have one instance, and provide a global point to access to it
"""

class Singleton(type):
    """
    Define an instance operation that lets client access its unique instance
    """
    def __init__(cls, name, bases, attrs, **kwargs):
        super().__init__(name, bases, attrs)
        cls._instance = None

    def __call__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__call__(*args, **kwargs)
        return cls._instance

class MyClass(metaclass=Singleton):
    pass


def main():
    m1 = MyClass()
    m2 = MyClass()
    assert m1 is m2

if __name__ == '__main__':
    main()
    
```

## example

```
class Singleton(type):

    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]


class MyClass(object):
    __metaclass__ = Singleton

def main():
    m1 = MyClass()
    m2 = MyClass()
    assert m1 is m2


if __name__ == '__main__':
    main()
```

## refs

https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
