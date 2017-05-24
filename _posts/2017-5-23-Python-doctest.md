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

## exception
```python
def update(self, timestamp, price):
    """Updates the stock with the price at the given timestamp

    >>> from datetime import datetime
    >>> stock = Stock("GOOG")
    >>> stock.update(datetime(2014, 10, 2), 10)
    >>> stock.price
    10

    The method raises a ValueError exception if the price is negative

    >>> stock.update(datetime(2014, 10, 2), -1)
    Traceback (most recent call last):
        ...
    ValueError: price should not be negative
    """

    if price < 0:
        raise ValueError("price should not be negative")
    self.history.update(timestamp, price)
    self.updated.fire(self)
```

## package-level doctests

in __init__.py

```python
r"""
The stock_alerter module allows you to set up rules and get started when those rules are met.

>>> from datetime import datetime

First, we need to setup an exchange that contains all the stocks that are going to be processed.
A simple dictionary will do.

>>> from stock_alerter.stock import Stock
>>> exchange = {"GOOG": Stock("GOOG"), "AAPL", Stock("AAPL")}

Next, we configure the reader. The reader is the source from where
the stock updates are coming. The module provides two readers out of
the box: A FileReader for reading updates from a comma separated
file, and a ListReader to get updates from a list. You can create
other readers, such as an HTTPReader, to get updates from a remote
server.
Here we create a simple ListReader by passing in a list of
3-tuples containing the stock symbol, timestamp and price.

>>> from stock_alerter.reader import ListReader
>>> reader = ListReader([("GOOG", datetime(2014, 2, 8), 5)])

Next, we set up an Alert. We give it a rule, and an action to be
taken when the rule is fired.

>>> from stock_alerter.alert import Alert
>>> from stock_alerter.rule import PriceRule
>>> from stock_alerter.action import PrintAction
>>> alert = Alert("GOOG > $3", PriceRule("GOOG",
lambda s: s.price > 3),\
...               PrintAction())

Connect the alert to the exchange

>>> alert.connect(exchange)

Now that everything is setup, we can start processing the updates

>>> from stock_alerter.processor import Processor
>>> processor = Processor(reader, exchange)
>>> processor.process()

GOOG > $3
"""
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

```
python -m stock_alerter.__init__
```

## doctest in files
```
if __name__ == "__main__":
    import doctest
    doctest.testfile("readme.txt")
```

## suite

```python
import doctest
import unittest
from stock_alerter import stock
class PackageDocTest(unittest.TestCase):
    def test_stock_module(self):
        doctest.testmod(stock)
    def test_doc(self):
        doctest.testfile(r"..\readme.txt")
```

```python
import doctest
from stock_alerter import stock
def load_tests(loader, tests, pattern):
    tests.addTests(doctest.DocTestSuite(stock))
    tests.addTests(doctest.DocFileSuite("../readme.txt"))
    return tests
```

```python
tests.addTests(doctest.DocFileSuite("readme.txt", package="stock_alerter"))
```

## fixture

```python
import doctest
from datetime import datetime
from stock_alerter import stock
def load_tests(loader, tests, pattern):
    tests.addTests(doctest.DocTestSuite(stock, globs={
        "datetime": datetime,
        "Stock": stock.Stock
    }))
    tests.addTests(doctest.DocFileSuite("readme.txt",
        package="stock_alerter"))
return tests
```
```python
import doctest
from datetime import datetime
from stock_alerter import stock

def setup_stock_doctest(doctest):
    s = stock.Stock("GOOG")
    doctest.globs.update({"stock": s})

def load_tests(loader, tests, pattern):
    tests.addTests(doctest.DocTestSuite(stock, globs={
        "datetime": datetime,
        "Stock": stock.Stock
    }, setUp=setup_stock_doctest))
    tests.addTests(doctest.DocFileSuite("readme.txt",
        package="stock_alerter"))
return tests
```
## directives

```python
>>> for key in sorted(exchange.keys()): #doctest: +ELLIPSIS
...    print(key, exchange[key])
...
AAPL <stock_alerter.stock.Stock object at 0x0...>
GOOG <stock_alerter.stock.Stock object at 0x0...>
```


```python
>>> for key in sorted(exchange.keys()):
... print(key, exchange[key])
... #doctest: +ELLIPSIS
... #doctest: +NORMALIZE_WHITESPACE
AAPL <stock_alerter.stock.Stock object at 0x0...>
GOOG <stock_alerter.stock.Stock object at 0x0...>
```

```python
>>> for key in sorted(exchange.keys()):
... print(key, exchange[key])
... #doctest: +ELLIPSIS, +NORMALIZE_WHITESPACE
AAPL    <stock_alerter.stock.Stock object at 0x0...>
GOOG    <stock_alerter.stock.Stock object at 0x0...>
```

**suite**
```python
options = doctest.ELLIPSIS | doctest.NORMALIZE_WHITESPACE
tests.addTests(doctest.DocFileSuite("readme.txt", package="stock_alerter", optionflags=options))
```
**minue**
```python
>>> for key in sorted(exchange.keys()):
...    print(key, exchange[key])
... #doctest: -NORMALIZE_WHITESPACE
AAPL <stock_alerter.stock.Stock object at 0x0...>
GOOG <stock_alerter.stock.Stock object at 0x0...>
```

* NORMALIZE_WHITESPACE
* IGNORE_EXCEPTION_DETAIL
* SKIP
* REPORT_ONLY_FIRST_FAILURE

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
