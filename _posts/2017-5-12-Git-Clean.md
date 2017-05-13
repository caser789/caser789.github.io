---
layout: post
title: Git Clean
date:   2017-05-12 09:54:01 +0800
categories: git clean gc notes filter-branch rev-list bisect rerere
tag: git
---

* content
{:toc}

Git Clean
------------------------

## clean

```
git clean -f
git clean -f untracked1.txt
git clean -d fold1
git clean -X -d fold1  # only file in ingore will be clean
git clean -x -d fold1  # both file in ingore and not in will be clean
git clean -i -x -d fold1
git clean -n # dry run
```

```
git config --global clean.requireForce false
```

# gc

```
git gc [--aggressive] [--auto] [--quiet] [--prune=<date> | --no-prune] [--force]
```

# notes

```
git notes 
git notes add -m "This is an example of a note" 2f2ea1e
git notes show 2f2ea1e
git notes --ref=review add -m "Looks ok to me" f3b05f9  # specific a namespace
git log --show-notes=*  # specific a namespace
```

# filter-branch
```
git filter-branch -f --subdirectory-filter web -- --all
```
# rev-list
```
git filter-branch --index-filter 'git rm --cached --ignore-unmatch <relative path to file>' <branch name>
git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch tmp_pass .txt' fix_secure
git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch tmp_pass .txt' HEAD
git filter-branch -f --index-filter 'git rm -r --cached --ignore-unmatch temp' fix_secure
git filter-branch -f --env-filter 'GIT_AUTHOR_EMAIL=bcl@mycompany.com; export GIT_AUTHOR_EMAIL' -- HEAD~3..HEAD
```
# bisect
```
git bisect reset # to end a bisect
git bisect reset HEAD
git bisect start HEAD HEAD~10
git bisect bad
git checkout HEAD~10
git bisect good
git bisect visualize
git bisect log
git bisect run ./sum.sh -2 2
```
# rerere

reuse recorded resolution

```
git config --global rerere.enabled 1
```
```
git rerere status
git rerere diff
ls .git/rr-cache

git rerere forget helloWorkshop.java
```
```
git log --follow --name-only temp/tmp_pass.txt
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
