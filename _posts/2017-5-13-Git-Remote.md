---
layout: post
title: Git Remote
date:   2017-05-13 09:54:01 +0800
categories: git remote clone view upstream push fetch pull
tag: git
---

* content
{:toc}

Git Remote
------------------------

## remote
```
git remote -v
git remote add version2 https://abc
git remote rename version2 origin2
git remote rm version2
```

# clone
```
git clone --bare
git clone --branch
```

# view

```
git branch -r
git branch -a
git branch -av
git branch -avv
git branch -rvv
git remote show origin
cat .git/config
```

# upstream

```
git branch test origin/test
git branch --track test origin/test
git branch --no-track test origin/test
git branch --set-upstream-to=origin/test test  # == -u
```

# push
```
git push
git push origin :
git push origin HEAD
git push origin lbranch1:rbranch1 lbranch2:rbranch2 lbranch3:rbranch3
git push <remote> <local branchname>:<remote branchname>
git push --all
git push --delete origin testing   # == git push origin :testing
git push --tags
git push --follow-tags
git push -f origin master  # == git push origin +master
```

# fetch
```
git fetch origin master:ui
git fetch -f origin master:ui
git fetch origin +master:ui
git merge origin/master
```

# pull
```
git pull origin +features:docs
git pull --rebase
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
