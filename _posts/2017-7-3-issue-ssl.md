---
layout: post
title: issue about ssl in python 2.7
date:   2017-07-03 02:18:01 +0000
categories: issue python ssl
tag: ssl
---

* content
{:toc}


## DESC

[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed

## Debug

和旧版本比较，发现requests升级到了2.18.1，增加了certifi和urllib3

把这两个包删除，其他的都恢复到旧版本，问题解决


## Reason

查看了下文档，应该是Python 升级到 2.7.9 之后引入了一个新特性，当你urllib.urlopen一个 https 的时候，会验证一次 SSL 证书

https://www.python.org/dev/peps/pep-0476/

instame服务器用的是python2.7.6版本，不知道为什么升级requests版本后也会出现这个问题。网上给出了一个可用的解决版本，就是在requests post或者get https的时候，传入参数verify=False，但不建议这么做。

## Fix

在ubuntu执行 `sudo apt-get install libffi-dev libssl-dev`

问题解决，把python版本升级到2.7.12以后，也没有出现ssl的错误。



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
