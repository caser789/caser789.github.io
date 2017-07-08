---
layout: post
title: A simple S-expression parser
date:   2017-07-08 08:18:01 +0000
categories: pyparsing parse python s-expression
tag: parse
---

* content
{:toc}

## BNF

```
alphaword ::= alphas+
integer ::= nums+
sexp ::= alphaword | integer | '(' sexp* ')'
```

## code

```python
from pyparsing import Word
from pyparsing import alphas
from pyparsing import nums
from pyparsing import Forward
from pyparsing import Suppress
from pyparsing import ZeroOrMore
from pyparsing import Group


alphaword = Word(alphas)
integer = Word(nums)

LPAREN = Suppress("(")
RPAREN = Suppress(")")
sexp = Forward()
#  sexp << (alphaword | integer | (LPAREN + ZeroOrMore(sexp) + RPAREN))
sexp << (alphaword | integer | Group(LPAREN + ZeroOrMore(sexp) + RPAREN))

tests = """\
    read
    100
    (red 100 blue)
    (green ((1 2) mauve) plaid())""".splitlines()

for t in tests:
    print t
    print sexp.parseString(t)
    print
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
