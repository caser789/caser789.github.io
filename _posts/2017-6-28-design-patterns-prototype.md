---
layout: post
title: Design patterns prototype
date:   2017-06-28 12:18:01 +0000
categories: pattern design prototype
tag: pattern
---

* content
{:toc}


## 目标

1. 通过复制自己创建新的 object

## 针对的问题

1. clone 比 new 快


## 其他

1. clone()
2. a registry

## 与其他模式关系

1. factory method 通过继承来 create, prototype 通过 delegation 来 create
2. Factory method >> abstract factory >> prototype >> builder
3. prototype 不需要 subclassing 但是需要 initialize, factory method 需要 subclassing，不需要 initialize
4. prototype 不需要 class 就可以创建 object

## 实现

```
"""
Specify the kinds of objects to create using a prototypical instance,
and create new objects by copying this prototype.
"""
import copy

class Prototype:
    """
    Example class to be copied.
    """
    pass

def main():
    prototype = Prototype()
    prototype_copy = copy.deepcopy(prototype)


if __name__ == '__main__':
    main()
```

## example

```
import copy


class WeeklyLog(object):

    def __init__(self, name, date, content):
        self.name = name
        self.date = date
        self.content = content

    def __eq__(self, other):
        return (
            self.name == other.name and
            self.date == other.date and
            self.content == other.content
        )

    def clone(self):
        return copy.deepcopy(self)


def main():
    wl = WeeklyLog('jiao.xue', '1st week', 'I\'m on fire!')
    print wl.name
    print wl.date
    print wl.content

    wl2 = wl.clone()
    assert wl == wl2
    assert not (wl is wl2)

    wl2.date = '13th week'
    print wl2.name
    print wl2.date
    print wl2.content


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
