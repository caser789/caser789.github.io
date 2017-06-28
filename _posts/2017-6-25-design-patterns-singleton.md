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


## examples

1. Windows task manager


## 其他

eager vs lazy


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

### windows task manager
```
class Singleton(type):

    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]


class TaskManager(object):
    __metaclass__ = Singleton

    def get_processes(self):
        print 'a lot of processes'

    def get_services(self):
        print 'loads of servies'


def main():
    m1 = TaskManager()
    m2 = TaskManager()
    assert m1 is m2
    m1.get_processes()
    m2.get_processes()
    m1.get_services()
    m2.get_services()


if __name__ == '__main__':
    main()
```

### load balancer

```python
import random


class Singleton(type):

    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]


class LoadBalancer(object):

    __metaclass__ = Singleton

    def __init__(self):
        self.servers = set()

    def add_server(self, server):
        self.servers.add(server)

    def remove_server(self, server):
        self.servers.remove(server)

    def get_server(self):
        if self.servers:
            # return random.sample((self.servers), 1)[-1]
            return random.choice(tuple(self.servers))


def main():
    lb1 = LoadBalancer()
    lb2 = LoadBalancer()
    lb3 = LoadBalancer()
    lb4 = LoadBalancer()

    assert lb1 is lb2
    assert lb3 is lb2
    assert lb4 is lb2

    lb1.add_server('Server 1')
    lb1.add_server('Server 2')
    lb1.add_server('Server 3')
    lb1.add_server('Server 4')

    for i in range(20):
        print 'dispatch request to %s' % lb1.get_server()

if __name__ == '__main__':
    main()
```

## refs

https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
