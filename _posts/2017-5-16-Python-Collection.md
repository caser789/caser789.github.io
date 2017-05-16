---
layout: post
title: Python Collections
date:   2017-05-16 13:57:01 +0800
categories: python collection iterable
tag: collection
---

* content
{:toc}

## process iterables
```
def _flatten(self, timestamps):
    for timestamp in timestamps:
        if not isinstance(timestamp, collections.Iterable):
            yield timestamp
        else:
            for value in self._flatten(timestamp):
                yield value
```
## refs

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
