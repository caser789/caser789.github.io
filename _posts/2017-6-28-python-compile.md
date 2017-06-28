---
layout: post
title: python compile
date:   2017-06-28 09:18:01 +0000
categories: python source compile dis
tag: compile
---

* content
{:toc}

## compile

```python
>>> compile("id('foo')", '<stdin>', 'single').co_consts
('foo', None)
```

##

```
>>> import dis
>>> dis.dis(compile("id('foo')", '<stdin>', 'single'))
  1           0 LOAD_NAME                0 (id)
              3 LOAD_CONST               0 ('foo')
              6 CALL_FUNCTION            1
              9 PRINT_EXPR          
             10 LOAD_CONST               1 (None)
             13 RETURN_VALUE        
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
