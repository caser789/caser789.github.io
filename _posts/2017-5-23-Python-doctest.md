---
layout: post
title: doctest
date:   2017-05-23 12:52:01 +0100
categories: python test doctest
tag: doctest
---

* content
{:toc}

## fundamental

```python
def price(self):
    """Returns the current price of the Stock

    >>> from datetime import datetime
    >>> stock = Stock("GOOG")
    >>> stock.update(datetime(2011, 10, 3), 10)
    >>> stock.price
    10
    """
    try:
        return self.history[-1].value
    except IndexError:
        return None


if __name__ == '__main__':
    import doctest
    doctest.testmod()
```

## run
```
python -m stock_alerter.stock -v
```
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
