---
layout: post
title: Setter and Getter in Python
date:   2017-05-16 15:56:01 +0800
categories: python setter getter property
tag: python
---

* content
{:toc}


## Way 1
```python
from decimal import Decimal

class Fees(object):
    """"""
    def __init__(self):
        """Constructor"""
        self._fee = None

    def get_fee(self):
        """
        Return the current fee
        """
        return self._fee

    def set_fee(self, value):
        """
        Set the fee
        """
        if isinstance(value, str):
            self._fee = Decimal(value)
        elif isinstance(value, Decimal):
            self._fee = value

    fee = property(get_fee, set_fee)
```
## Way 2
```python
from decimal import Decimal


class Fees(object):
    """"""
    def __init__(self):
        """Constructor"""
        self._fee = None

    @property
    def fee(self):
        """
        The fee property - the getter
        """
        return self._fee

    @fee.setter
    def fee(self, value):
        """
        The setter of the fee property
        """
        if isinstance(value, str):
            self._fee = Decimal(value)
        elif isinstance(value, Decimal):
            self._fee = value

if __name__ == "__main__":
    f = Fees()
    f.set_fee("1")
    print f.fee # Decimal('1')
    f.fee = "2"
    print f.get_fee() #Decimal('2')
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
