---
layout: post
title: Design patterns proxy
date:   2017-06-24 16:18:01 +0800
categories: pattern design proxy
tag: pattern
---

* content
{:toc}


## 目标

1. 占位对真是对象的访问
2. 限制权限，或者添加一层以提供分布式，或者受控的访问
3. 保护真实对象

## 针对的问题

对象的创建十分消耗资源，需要等到真正有客户端的连接才创建对象

## 4种场景

1. virtual proxy：根据需要创建开销很大的对象。通过它来存放实例化需要很长事件的真实对象。
2. remote proxy：为一个对象在不同的地址空间提供局部代表。隐藏一个对象存在于不同地址空间的事实。
3. protective proxy: 限制权限，在方法中检验身份
4. smart proxy：在方法中处理另外的事
    * 处理引用计数
    * 懒加载持久化的对象
    * 检查资源的锁

## 其他

可以考虑通过一个工厂模式，可以选择使用代理还是真实对象

## 与其他模式关系

1. Adapter 给对象增加不同的接口；Decorator 给对象增加增强版的接口；Proxy
   给对象增加一样的接口


## 实现

```
"""
Provide a surrogate or placeholder for another object to control access to it
or add other responsibilities.
"""

import abc

class Subject(metaclass=abc.ABCMeta):
    """
    Define the common interface for RealSubject and Proxy so that a proxy can
    be anywhere a RealSubject is expected.
    """
    @abc.abstractmethod
    def request(self):
        pass


class Proxy(Subject):
    """
    Maintain a reference that lets the proxy access the real subject. 
    Provide an interface identical to Subject's
    """
    def __init__(self, real_subject):
        self._real_subject = real_subject

    def request(self):
        # ...
        self._real_subject.request()
        # ...

class RealSubject(Subject):
    """
    Define the real subject that the proxy represents
    """
    def request(self):
        pass

def main():
    real_subject = RealSubject()
    proxy = Proxy(real_subject)
    proxy.request()


if __name__ == '__main__':
    main()
```

## example

```
import abc


class Base(object):

    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def pursue(self, pursuee):
        pass


class Pursuee(object):

    def __init__(self, name):
        self.name = name


class Pursuer(Base):

    def __init__(self, name):
        self.name = name

    def pursue(self, pursuee):
        print 'I love you, {pursuee.name}'.format(pursuee=pursuee)


class Proxy(Base):

    def __init__(self, pursuer):
        self.pursuer = pursuer

    def pursue(self, pursuee):
        print "I'm a proxy for {pursuer.name}, and He says: ".format(pursuer=self.pursuer)
        self.pursuer.pursue(pursuee)


def main():
    yan = Pursuee('yan')
    jiao = Pursuer('jiao')
    proxy = Proxy(jiao)
    proxy.pursue(yan)


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
