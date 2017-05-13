---
layout: post
title: Git Subtree
date:   2017-05-13 11:54:01 +0800
categories: git subtree 
tag: git
---

* content
{:toc}

Git Subtree
------------------------

## subcommands
```
git subtree add -p <prefix> <commit>
git subtree add -p <prefix> <repository> <ref>
git subtree pull -p <prefix> <repository> <ref>
git subtree push -p <prefix> <repository> <ref>
git subtree merge -p <prefix> <commit>
git subtree split -p <prefix> [OPTIONS] [<commit>]
```
## add
```
git subtree add --prefix subproject ~/subtree/remotes/subproj.git master

git remote add sub_origin ~/subtrees/remotes/subproj.git
git subtree add --prefix subproject --squash sub_origin master
```

## update

```
git subtree pull --prefix subproject sub_origin master --squash
```

## split
```
git subtree split --prefix=subproject --branch=split_branch
```

## push
```
git subtree push --prefix=subproject sub_origin new_branch
```

## Create a new project
```
mkdir newproj
cd newproj
git init
git pull ~/subtrees/local/myproject split_branch
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
