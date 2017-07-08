---
layout: post
title: Parsing a search string
date:   2017-07-08 12:28:01 +0000
categories: pyparsing parse python search
tag: parse
---

* content
{:toc}


## samples

```
wood and blue or red
wood and (blue or red)
(steel or iron) and "lime green"
not steel or iron and "lime green"
not(steel or iron) and "lime green"
```

## BNF

```
search_expression ::= search_term [ (AND | OR) search_term]...
search_term ::= [NOT] (single_word | quoted_string | '(' search_expression ')')
```
## precedence

### BNF
with precedence
```
search_expression ::= search_and [OR search_and]...
search_and ::= search_term [AND search_term]...
search_term ::= [NOT] (single_word | quoted_string | '(' search_expression ')')
```



### grammar

```
search_expression = Forward()
search_term = Optional(not_) + (Word(alphas) | quoted_string.setParseAction(removeQuotes) | Group(LPAREN + search_expression + RPAREN))
search_and = Group(search_term + ZeroOrMore(and_ + search_term))
search_expression << Group(search_and + ZeroOrMore(or_ + search_and))
```

### operatorPrecedence method


```
# the smallest operand
search_term = Word(alphas) + quoted_string.setParseAction(removeQuotes)

search_expression = operatorPrecedence(search_term,
    [
        (not_, 1, opAssoc.RIGHT),
        (and_, 2, opAssoc.LEFT),
        (or_, 2, opAssoc.LEFT),
    ]
)
```

## code

```python
from pyparsing import operatorPrecedence
from pyparsing import opAssoc
from pyparsing import Word
from pyparsing import alphanums
from pyparsing import removeQuotes
from pyparsing import CaselessLiteral
from pyparsing import quotedString

and_ = CaselessLiteral("and")
or_ = CaselessLiteral("or")
not_ = CaselessLiteral("not")

search_term = Word(alphanums) | quotedString.setParseAction(removeQuotes)
search_expression = operatorPrecedence(
    search_term,
    [
        (not_, 1, opAssoc.RIGHT),
        (and_, 2, opAssoc.LEFT),
        (or_, 2, opAssoc.LEFT),
    ]
)


tests = '''\
    wood and blue or red
    wood and (blue or red)
    (steel or iron) and "lime green"
    not steel or iron and "lime green"
    not(steel or iron) and "lime green"'''.splitlines()
for t in tests:
    print t.strip()
    print search_expression.parseString(t)[0]
    print
```
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
