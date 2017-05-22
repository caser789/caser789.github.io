---
layout: post
title: pdb in python
date:   2017-05-22 06:32:01 +0100
categories: python test pdb ipdb
tag: pdb
---

* content
{:toc}

## Use in shell
```
>>> import pdb
>>> from stock_alerter.legacy import AlertProcessor
>>> pdb.run("AlertProcessor()")
> <string>(1)<module>()
(Pdb)
```

## options
```
(Pdb) help
help s
```

## common cmds

* s: This executes one line of code (going inside a function call if required)
* n: This executes code until you reach the next line in the current function
* r: This executes code until the current function returns
* q: This quits the debugger
* b: This sets a breakpoint on a particular line of a file
* cl: This clears breakpoints
* c: This continues execution until a breakpoint is encountered or until the end of execution
* pp: pretty print


## set breaking point
```
(Pdb) b stock_alerter\legacy.py:25
(Pdb) c
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
