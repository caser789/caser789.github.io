---
layout: post
title: nose2
date:   2017-05-24 13:41:01 +0100
categories: python test nose2 unittest
tag: nose2
---

* content
{:toc}


## install
```
pip install nose2
```

## unitetest no more
**unittest**
```
class StockTest(unittest.TestCase):
    def setUp(self):
        self.goog = Stock("GOOG")
    def test_price_of_a_new_stock_class_should_be_None(self):
        self.assertIsNone(self.goog.price)
```
**nose2**
```
def test_price_of_a_new_stock_class_should_be_None():
    goog = Stock("GOOG")
    assert goog.price is None, "message"
```

## Setup and teardown
```
def setup_test():
    global goog
    goog = Stock("GOOG")

def teardown_test():
    global goog
    goog = None

def test_price_of_a_new_stock_class_should_be_None():
    assert goog.price is None, "Price of a new stock should be None"

test_price_of_a_new_stock_class_should_be_None.setup = setup_test
test_price_of_a_new_stock_class_should_be_None.teardown = teardown_test
```

## params (TDD)
```
from nose2.tools.params import params
def given_a_series_of_prices(stock, prices):
  timestamps = [datetime(2014, 2, 10), datetime(2014, 2, 11),
                  datetime(2014, 2, 12), datetime(2014, 2, 13)]
    for timestamp, price in zip(timestamps, prices):
        stock.update(timestamp, price)

@params(
    ([8, 10, 12], True),
    ([8, 12, 10], False),
    ([8, 10, 10], False)
)
def test_stock_trends(prices, expected_output):
    goog = Stock("GOOG")
    given_a_series_of_prices(goog, prices)
    assert goog.is_increasing_trend() == expected_output
```

## Generated tests
```
def test_trend_with_all_consecutive_values_upto_100():
    for i in range(100):
        yield stock_trends_with_consecutive_prices, [i, i+1, i+2]
def stock_trends_with_consecutive_prices(prices):
    goog = Stock("GOOG")
    given_a_series_of_prices(goog, prices)
    assert goog.is_increasing_trend()
```

## layers

```
from nose2.tools import such
with such.A("Stock class") as it:

    @it.has_setup
    def setup():
        it.goog = Stock("GOOG")

    with it.having("a price method"):

        @it.has_setup
        def setup():
            it.goog.update(datetime(2014, 2, 12), price=10)

        @it.should("return the price")
        def test(case):
            assert it.goog.price == 10

        @it.should("return the latest price")
        def test(case):
            it.goog.update(datetime(2014, 2, 11), price=15)
            assert it.goog.price == 10

    with it.having("a trend method"):

        @it.should("return True if last three updates were increasing")
        def test(case):
            it.goog.update(datetime(2014, 2, 11), price=12)
            it.goog.update(datetime(2014, 2, 12), price=13)
            it.goog.update(datetime(2014, 2, 13), price=14)
            assert it.goog.is_increasing_trend()

    it.createTests(globals())
```
```
nose2 --plugin nose2.plugins.layers
nose2 --plugin nose2.plugins.layers --layer-reporter -v
```

## Plugins

```
nose2 --plugin nose2.plugins.doctests --with-doctest
nose2 --plugin nose2.plugins.junitxml --junit-xml
```

## Coverage
```
pip install nose2[coverage-plugin]
nose2 --with-coverage
nose2 --with-coverage --coverage-report html
nose2 --with-coverage --coverage-report html --coverage-report term
```
## debug
```
nose2 --plugin nose2.plugins.debugger --debugger
```

## config

```
nose2 --with-coverage --coverage-report html --plugin nose2.plugins.junitxml --junit-xml --plugin nose2.plugins.doctests --with-doctest
```

**nose2.cf



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
