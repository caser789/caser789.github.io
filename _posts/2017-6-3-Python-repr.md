---
layout: post
title: Usage example of repr in python
date:   2017-06-03 03:18:01 +0100
categories: python repr magic
tag: repr
---

* content
{:toc}

```python
class Variable(object):
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return "Variable({0.name})".format(self)

    def __repr__(self):
        return "Variable({0.name!r})".format(self)
```


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
