---
layout: post
title: Pattern: strategy
date:   2017-06-22 15:18:01 +0100
categories: pattern strategy
tag: pattern
---

* content
{:toc}

## simple strategy

```python
class StrategyExample:

    def __init__(self, func=None):
        if func:
            self.execute = func

    def execute(self):
        print "Original execution"


def executeReplacement1():
    print "Strategy 1"


def executeReplacement2():
    print "Strategy 2"

if __name__ == '__main__':
    strat0 = StrategyExample()
    strat1 = StrategyExample(executeReplacement1)
    strat2 = StrategyExample(executeReplacement2)

    strat0.execute()
    strat1.execute()
    strat2.execute()
```
### static way
above

cannot access self
### bind func to all instances
```
StrategyExample.execute = func
```
### bind only to instance
```
strat0.execute = types.MethodType(executeReplacement1, strat0, StrategyExample)
```

## access self in function

```python
import sys
import types

if sys.version_info[0] > 2:  # Python 3+
    create_bound_method = types.MethodType
else:
    def create_bound_method(func, obj):
        return types.MethodType(func, obj, obj.__class__)

class StrategyExample:
    def __init__(self, func=None):
        self.name = "Strategy Example 0"
        if func:
             self.execute = create_bound_method(func, self)

    def execute(self):
        print(self.name)

def executeReplacement1(self):
    print(self.name + " from execute 1")

def executeReplacement2(self):
    print(self.name + " from execute 2")

if __name__ == "__main__":
    strat0 = StrategyExample()
    strat1 = StrategyExample(executeReplacement1)
    strat1.name = "Strategy Example 1"
    strat2 = StrategyExample(executeReplacement2)
    strat2.name = "Strategy Example 2"

    strat0.execute()
    strat1.execute()
    strat2.execute()
```
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
