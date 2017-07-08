---
layout: post
title: pyparsing
date:   2017-07-08 03:18:01 +0000
categories: pyparsing parse python
tag: parse
---

* content
{:toc}


## example

```
from pyparsing import nums
from pyparsing import Combine
from pyparsing import Word
from pyparsing import alphas
from pyparsing import alphanums

ip_field = Word(nums, max=3)
ip_addr = Combine(ip_field + '.' + ip_field + '.' + ip_field + '.' + ip_field)
phone_num = Combine("(" + Word(nums, exact=3) + ")" + Word(nums, exact=3) + "-" + Word(nums, exact=4))
user_data = ip_addr + phone_num


# variable

identifier = Word(alphas, alphanums+'_')

# number, integer and floating point

number = Word(nums+".")

# assignment

assignment_expression = identifier + "=" + (identifier | number)

```

## parse methods

1. parseString
2. scanString
3. searchString
4. transformString


## assign name to result set

```
assignment_expression = identifier.setResultsName("lhs") + "=" + (identifier | number).setResultsName("rhs")

assignment_tokens = assignment_expression.parseString("pi=3.1415926")
print assignment_tokens.rhs, "is assigned to", assignment_tokens.lhs
```

## hello world

### text to parse
```
Hello, World!
Hi, Mom!
Good morning, Miss Crabtree!
Yo, Adrian!
Whattup, G?
How's it goin', Dude?
Hey, Jude!
Goodbye, Mr. Chips!
```

### BNF

```
greeting   ::= salutation comma greetee endpunc
salutation ::= word+
comma      ::= ,
greetee    ::= word+
word       ::= a collection of one or more characters, which are any alpha or ' or .
endpunc    ::= !|?
```

### pyparsing

```
word = Word(alphas + "'.")
salutation = OneOrMore(word)
comma = Literal(",")
greetee = OneOrMore(word)
endpunc = oneOf("! ?")
greeting = salutation + comma + greetee + endpunc
```
oneOf
```
endpunc = oneOf("! ?") # a single string of items seperated by whitespace
endpunc = Literal("!") | Literal("?")
```


### grouping

```
salutation = Group(OneOrMore(word))
greetee = Group(OneOrMore(word))
```

### Do not show , in the result set

```
comma = Suppress(Literal(","))
comma = Literal(",").suppress()
comma = Suppress(",")
```


## a table

### text
```
09/04/2004 Virginia      44  Temple              14
09/04/2004 LSU           22  Oregon State        21
09/09/2004 Troy State    24  Missouri            14
01/02/2003 Florida State 103 University of Miami 2
```
### BNF

```
digit ::= '0'..'9'
alpha ::= 'A'..'Z' 'a'..'z'
date ::= digit+ '/' digit+ '/' digit+
schoolName ::= ( alpha+ )+
score ::= digit+
schoolAndScore ::= schoolName score
gameResult ::= date schoolAndScore schoolAndScore
```

### parse

```
def validate_date(tokens):
    try:
        time.strptime(tokens[0], "%m/%d/%Y")
    except ValueError as e:
        raise ParseException("Invalid date string (%s)" % tokens[0])

num = Word(nums)
# date = num + '/' + num + "/" + num
date = Combine(num + '/' + num + "/" + num)
date.setParseAction(validate_date)
school_name = OneOrMore(Word(alphas))
school_name.setParseAction(lambda tokens: " ".join(tokens)) # Why not combine?
# score = Word(nums)
score = Word(nums).setParseAction(lambda tokens: int(tokens[0]))
# school_and_score = school_name + score
school_and_score = Group(school_name.setResultsName("school") + score.setResultsName("score"))
# game_result = date.setResultsName("date") + school_and_score.setResutlsName("team1") + school_and_score.setResultsName("team2")
game_result = date("date") + school_and_score("team1") + school_and_score("team2")
```


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
