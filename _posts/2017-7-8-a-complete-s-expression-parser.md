---
layout: post
title: A complete S-expression parser
date:   2017-07-08 08:28:01 +0000
categories: pyparsing parse python s-expression
tag: parse
---

* content
{:toc}

## BNF

```
<sexp> :: <string> | <list>

<string> :: <display>? <simple-string> ;
<display> :: "[" <simple-string> "]" ;
<simple-string> :: <raw> | <token> | <base-64> | <hexadecimal> | <quoted-string> ;
<raw> :: <decimal> ":" <bytes> ;
<token> :: <tokenchar>+ ;
<base-64> :: <decimal>? "|" ( <base-64-char> | <whitespace> )* "|" ;
<hexadecimal> :: "#" ( <hex-digit> | <white-space> )* "#" ;
<quoted-string> :: <decimal>? <quoted-string-body>
<decimal> :: <decimal-digit>+ ;
          -- decimal numbers should have no unnecessary leading zeros
          
<bytes>  -- any string of bytes, of the indicated length
<token-char> :: <alpha> | <decimal-digit> | <simple-punc> ;
<base-64-char> :: <alpah> | <decimal-digit> | "+" | "/" | "=" ;
<whitespace> :: <whitespace-char>* ;
<whitespace-char> :: " " | "\t" | "\r" | "\n" ;
<quoted-string-body> :: "\"" <bytes> "\""
<decimal-digit> :: "0" | ... | "9" ;
<hex-digit> :: <decimal-digit> | "A" | ... | "F" | "a" | ... | "f" ;
<simple-punc> :: "-" | "." | "/" | "_" | ":" | "*" | "+" | "=" ;
<upper-case> :: "A" | ... | "Z" ;
<lower-case> :: "a" | ... | "z" ;
<alpha> :: <upper-case> | <lower-case> | <digit> ;

<list> :: "(" ( <sexp> | <whitespace>)* ")";
```

## pyparsing elements

alphas
    The characters A-Z and a-z srange("[A-Z]")
nums
    The characters 0-9
alphanums
    The combination of alphas and nums
hexnums
    The combination of nums and the charactoers A-F and a-f
printables
    All 7-bit ASCII characters that are not whitespace or control characters

## remove quotes

```python
from pyparsing import dblQuotedString
dblQuotedString.setParseAction(removeQuotes)
quoted_string = Optional(decimal) + dblQuotedString
```
## convert b64 string to binary
```
from base64 import b64decode
base_64 = Optional(decimal) + "|" + ZeroOrMore(Word(base_64_char)).setParseAction(lambda t: b64decode("".join(t))) + "|"
```

## validate length of string

```

quoted_string = Optional(decimal.setResultsName('length')) + dblQuotedString.setResultsName("data")
raw = decimal.setResultsName("length") + ":" + bytes_.setResultsName("data")

base_64_body = OneOrMore(Word(base_64_char))
base_64_body.setParseAction(lambda t: b64decode("".join(t)))
base_64 = Optional(decimal).setResultsName("length") + "|" + base_64_body.setResultsName("data") + "|"
```

## code

```python
from pyparsing import alphanums
from pyparsing import OneOrMore
from pyparsing import Group
from pyparsing import removeQuotes
from pyparsing import Suppress
from pyparsing import Forward
from pyparsing import Word
from pyparsing import printables
from pyparsing import srange
from pyparsing import nums
from pyparsing import dblQuotedString
from pyparsing import Optional
from pyparsing import hexnums
from pyparsing import ZeroOrMore
from pyparsing import ParseFatalException
from base64 import b64decode
import pprint

# <base-64-char> :: <alpha> | <decimal-digit> | "+" | "/" | "=" ;
base_64_char = alphanums + "+/="

# <whitespace-char> :: " " | "\t" | "\r" | "\n" ;
# not needed

# <simple-punc> :: "-" | "." | "/" | "_" | ":" | "*" | "+" | "=" ;
simple_punc = "-./_:*+="

# <hex-digit> :: <decimal-digit> | "A" | ... | "F" | "a" | ... | "f" ;
# use hexnums

# <decimal-digit> :: "0" | ... | "9" ;
# use nums

# <alpha> :: <upper-case> | <lower-case> | <digit> ;
# <lower-case> :: "a" | ... | "z" ;
# <upper-case> :: "A" | ... | "Z" ;
# use alphanums

# <tokenchar> :: <alpha> | <decimal-digit> | <simple-punc> ;
tokenchar = alphanums + simple_punc

LPAREN, RPAREN, LBRACK, RBRACK = [Suppress(char) for char in "()[]"]

# <bytes> -- any string of bytes, of the indicated length
bytes_ = Word(printables)

# <decimal> :: <decimal-digit>+ ;
#           -- decimal numbers should have no unnecessary leading zeros
decimal = "0" | Word(srange("[1-9]"), nums)

# <quoted-string-body> :: "\"" <bytes> "\""
# not needed, use dbQuotedString

# <quoted-string> :: <decimal>? <quoted-string-body>
dblQuotedString.setParseAction(removeQuotes)
quoted_string = Optional(decimal.setResultsName('length')) + dblQuotedString.setResultsName("data")

# <hexadecimal> :: "#" (<hex-digit> | <white-space>)* "#" ;
hexadecimal = "#" + ZeroOrMore(Word(hexnums)) + "#"

# <base-64> :: <decimal>? "|" (<base-64-char> | <whitespace>)* "|" ;
base_64_body = OneOrMore(Word(base_64_char))
base_64_body.setParseAction(lambda t: b64decode("".join(t)))
base_64 = Optional(decimal).setResultsName("length") + "|" + base_64_body.setResultsName("data") + "|"

# <token> :: <tokenchar>+ ;
token = Word(tokenchar)

# <raw> :: <decimal> ":" <bytes> ;
raw = decimal.setResultsName("length") + ":" + bytes_.setResultsName("data")

# <simple-string> :: <raw> | <token> | <base-64> | <hexadecimal> | <quoted-string> ;
simple_string = raw | token | base_64 | hexadecimal | quoted_string

# <display> :: "[" <simple-string> "]" ;
display = LBRACK + simple_string + RBRACK

# <string> :: <display>? <simple-string> ;
string = Optional(display) + simple_string

# <list> :: "(" (<secp> | <whitespace>)* ")" ;
sexp = Forward()
list_ = LPAREN + Group(ZeroOrMore(sexp)) + RPAREN

# <sexp> :: <string> | <list>
sexp << (string | list_)


def validate_data_length(tokens):
    if tokens.length != "":
        if len(tokens.data) != int(tokens.length):
            raise ParseFatalException("Invalid data length, %d specified, found %s (%d chars)" % (int(tokens.length), tokens.data, len(tokens.data)))

quoted_string.setParseAction(validate_data_length)
base_64.setParseAction(validate_data_length)
raw.setParseAction(validate_data_length)


test0 = """(snicker "abc" (#03# |YWJj|))"""
test1 = """
(certificate
 (issuer
  (name
   (public-key
    rsa-with-md5
    (e |NFGq/E3wh9f4rJIQVXhS|)
    (n |d738/4ghP9rFZ0gAIYZ5q9y6iskDJwASi5rEQpEQq8ZyMZeIZzIAR2I5iGE=|))
   aid-committee))
 (subject
  (ref
   (public-key
    rsa-with-md5
    (e |NFGq/E3wh9f4rJIQVXhS|)
    (n |d738/4ghP9rFZ0gAIYZ5q9y6iskDJwASi5rEQpEQq8ZyMZeIZzIAR2I5iGE=|))
tom
   mother))
(not-after "1998-01-01_09:00:00")
 (tag
(spend (account "12345678") (* numeric range "1" "1000"))))"""
test2 = """\
    (defun factorial (x)
        (if (zerop x) 1
            (* x (factorial (- x 1)))))
    """
test3 = """(3:XX "abc" (#30# |YWJj|)"""

for t in (test0, test1, test2, test3):
    print '-' * 50
    print t
    try:
        sexpr = sexp.parseString(t)
        pprint.pprint(sexpr.asList())
    except ParseFatalException as pfe:
        print "Error: ", pfe
    print
```



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
