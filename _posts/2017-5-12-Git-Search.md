---
layout: post
title: Git Search
date:   2017-05-12 10:54:01 +0800
categories: git search config
tag: git
---

* content
{:toc}

Git Search
------------------------

## example

```
git grep [-a | --text] [-I] [--textconv] [-i | --ignore-case] [-w | --word- regexp]

git grep database -- *.java

git grep -p database -- *.java # show the context

git grep -n database -- *.java # line number

git grep -p --heading database -- *.java # filename as header
git grep -p --break database -- *.java # seperate matches with a blank line

git grep -e 'database' --and -e 'access' -- *.java
git grep -e 'database' --or -e 'access' -- *.java  # default
git grep -e 'database' -e 'access' -- *.java

git grep -e 'database' HEAD -- *.java
git grep -e 'database' b2e575a -- *.java

git grep -e 'config' --cached -- '*.txt'
```

# Config

```
git config grep.lineNumber true
```

# log search

```
git log -S "abc"   # match a string

git log --oneline --pickaxe-regex -S "line [1-3]"
git log --oneline -S "[Ll]ine 4" --pickaxe-regex
git log --oneline -G "[Ll]ine 4"

git log -L4,5:lines.txt
git log -L :<funcname>:<file>
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
