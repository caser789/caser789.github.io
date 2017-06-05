---
layout: post
title: Simple iterator parser in python
date:   2017-06-05 08:18:01 +0100
categories: python parser iterator
tag: parser
---

* content
{:toc}

```python
import cStringIO
import tokenize


def atom(next, token):
    if token[1] == "(":
        out = []
        token = next()
        while token[1] != ")":
            out.append(atom(next, token))
            token = next()
            if token[1] == ",":
                token = next()
        return tuple(out)
    elif token[0] is tokenize.STRING:
        return token[1][1:-1].decode("string-escape")
    elif token[0] is tokenize.NUMBER:
        try:
            return int(token[1], 0)
        except ValueError:
            return float(token[1])
    raise SyntaxError("malformed expression (%s)" % token[1])


def simple_eval(source):
    """
    >>> simple_eval("'hello'")
    'hello'
    >>> simple_eval("(1, 2, 3, 4711.0)")
    (1, 2, 3, 4711.0)
    >>> simple_eval("('spam', 123, ('spam', 'egg'), 0x10, (3.14,))")
    ('spam', 123, ('spam', 'egg'), 16, (3.1400000000001,))
    >>> simple_eval("'hello')))")
    Traceback (most recent call last):
    File "", line 1, in ?
    File "test.py", line 27, in simple_eval
        raise SyntaxError("bogus data after expression")
    SyntaxError: bogus data after expression
    """
    src = cStringIO.StringIO(source).readline
    src = tokenize.generate_tokens(src)
    src = (token for token in src if token[0] is not tokenize.NL)
    res = atom(src.next, src.next())
    if src.next()[0] is not tokenize.ENDMARKER:
        raise SyntaxError("bogus data after expression")
    return res
```

[A simple iterator parser](http://effbot.org/zone/simple-iterator-parser.htm)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
