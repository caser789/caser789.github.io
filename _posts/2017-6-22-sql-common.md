---
layout: post
title: Most used sqls
date:   2017-06-22 04:18:01 +0100
categories: sql mysql
tag: sql
---

* content
{:toc}


```
CREATE USER 'ohippo_rw'@'%' identified by 'abc';
CREATE DATABASE ohippo;
GRANT SELECT ON ohippo.* TO 'ohippo_rw'@'%';
GRANT INSERT ON ohippo.* TO 'ohippo_rw'@'%';
GRANT UPDATE ON ohippo.* TO 'ohippo_rw'@'%';
GRANT DELETE ON ohippo.* TO 'ohippo_rw'@'%';
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
