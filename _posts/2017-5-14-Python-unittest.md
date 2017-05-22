---
layout: post
title: Python Unittest
date:   2017-05-14 23:40:01 +0800
categories: python unittest
tag: unittest
---

* content
{:toc}

# python unittest
------------------------
## discover

```
python -m unittest discover
python -m unittest discover -s start_dir
python -m unittest discover -t top_dir
python -m unittest discover -p file_pattern
python -m unittest discover -p *_test.py
python -m unittest discover -s sub_dir -t current_dir
```

## test raise exception method 1
```
goog = Stock("GOOG")
try:
    goog.update(datetime(2014, 2, 12), -1)
except ValueError:
    return
self.fail("ValueError was not raised")
```
## test raise exception method 2
```
goog = Stock("GOOG")
self.assertRaises(ValueError, goog.update, datetime(2014, 2, 12), -1)
```

## test raise exception method 3
```
goog = Stock("GOOG")
with self.assertRaises(ValueError):
    goog.update(datetime(2014, 2, 12), price=-2)
```

## test float equal
```
self.assertAlmostEqual(8.4, goog.price, delta=0.001)
self.assertAlmostEqual(8.4, goog.price, places=4)
```

## context
```
import unittest

def setUpModule():
    print "setUpModule"

def tearDownModule():
    print "tearDownModule"

class Class1Test(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        print "    setUpClass"

    @classmethod
    def tearDownClass(cls):
        print "    tearDownClass"

    def setUp(self):
        print "        setUp"

    def tearDown(self):
        print "        tearDown"

    def test_1(self):
        print "            class 1 test 1"

    def test_2(self):
        print "            class 1 test 2"

class Class2Test(unittest.TestCase):
    def test_1(self):
        print "            class 2 test 1"
```

## cheatsheet

```
self.assertFalse(x, msg)
self.assertTrue(x, msg)

self.assertIsNone(x, msg)
self.assertIsNotNone(x, msg)

self.assertEqual(x, y, msg)
self.assertNotEqual(x, y, msg)

self.assertAlmostEqual(x, y, places, msg, delta)
self.assertNotAlmostEqual(x, y, places, msg, delta)

self.assertGreater(x, y, msg)
self.assertGreaterEqual(x, y, msg)

self.assertLess(x, y, msg)
self.assertLessEqual(x, y, msg)

self.assertIs(x, y, msg)
self.assertIsNot(x, y, msg)

self.assertIn(x, seq, msg)
self.assertNotIn(x, seq, msg)

self.assertIsInstance(x, cls, msg)
self.assertNotIsInstance(x, cls, msg)

self.assertRegex(text, regex, msg)
self.assertNotRegex(text, regex, msg)

self.assertRaises(exception, callable, *args, **kw)

self.fail(msg)
```

## addCleanup

```
class SomeTest2(unittest.TestCase):
    def setUp(self):
        connect_to_db()
        self.addCleanup(self.disconnect_db)
        connect_to_server()
        self.addCleanup(self.disconnect_server)

    def disconnect_db(self):
        disconnect_from_db()

    def disconnect_server(self):
        disconnect_from_server()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
