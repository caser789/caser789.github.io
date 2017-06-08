---
layout: post
title: Python decorator
date:   2017-06-08 09:18:01 +0100
categories: python decorator
tag: decorator
---

* content
{:toc}

## assign function as variables

```
def greet(name):
    return "Hello {0}".format(name)

greet_someone = greet
print greet_someone("John")
```

## define function inside other functions

```
def greet(name):
    def get_message():
        return "Hello "
    result = get_message() + name
    return result

print greet("Jhon")
```

## function as parameter

```
def greet(name):
    return "Hello {0}".format(name)


def call_func(func):
    name = "John"
    return func(name)

print call_func(greet)
```

## function return other functions

```
def compose_greet_func():
    def get_message():
        return "Hello there"
    return get_message


greet = compose_greet_func()
print greet()
```

## closure

```
def compose_greet_func(name):
    def get_message():
        return "Hello {0}".format(name)
    return get_message


greet = compose_greet_func("John")
print greet()
```

## decorator

```
def get_text(name):
    return "lorem ipsum, {0} dolor".format(name)


def p_decorate(func):
    def func_wrapper(name):
        return "<p>{0}</p>".format(func(name))
    return func_wrapper


my_get_text = p_decorate(get_text)
print my_get_text("John")
```

[ref](https://www.thecodeship.com/patterns/guide-to-python-function-decorators/)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
