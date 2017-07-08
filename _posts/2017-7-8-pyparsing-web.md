---
layout: post
title: pyparsing parse web
date:   2017-07-08 06:18:01 +0000
categories: pyparsing parse python web
tag: parse
---

* content
{:toc}

##  code

```python
from pyparsing import makeHTMLTags
from pyparsing import nums
from pyparsing import Word
from pyparsing import Optional
from pyparsing import OneOrMore
from pyparsing import SkipTo

import urllib

url = "https://www.cia.gov/library/publications/the-world-factbook/appendix/appendix-g.html"

html = urllib.urlopen(url).read()

decimal_number = Word(nums, nums+",") + Optional("." + OneOrMore(Word(nums)))
decimal_number.setParseAction(
    lambda tokens: "".join(tokens),
    lambda tokens: tokens[0].replace(",", ""),
    lambda tokens: float(tokens[0])
)

tests = """\
200
0.032 808 40
1,728
0.028 316 846 592
3,785.411 784""".splitlines()
for test in tests:
    print decimal_number.parseString(test)

td_start, td_end = makeHTMLTags("td")
conversion_value = td_start + decimal_number.setResultsName("factor") + td_end
for tokens, start, end in conversion_value.scanString(html):
    print tokens.factor

units = SkipTo(td_end).setParseAction(
    lambda tokens: tokens[0].replace("<br>", " "),
    lambda tokens: tokens[0].replace("<br />", " "),
    lambda tokens: " ".join(tokens[0].split())
)
from_unit = td_start + units.setResultsName("fromUnit") + td_end
to_unit = td_start + units.setResultsName("toUnit") + td_end

tr_start, tr_end = makeHTMLTags("tr")
conversion = tr_start + from_unit + to_unit + conversion_value + tr_end

for tokens, start, end in conversion.scanString(html):
    print "{tokens.fromUnit}: {tokens.toUnit} : {tokens.factor}".format(tokens=tokens)
```

## BNF

```
entry ::= <tr> conversionLabel conversionLabel conversionValue </tr>
conversionLabel ::= <td> text </td>
conversionValue ::= <td> readableNumber </td>
```


## numbers format

```
200
0.032 808 40
1,728
0.028 316 846 592
3,785.411 784
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
