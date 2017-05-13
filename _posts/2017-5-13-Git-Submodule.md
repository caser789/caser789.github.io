---
layout: post
title: Git Submodule
date:   2017-05-13 10:54:01 +0800
categories: git worktree submodule
tag: git
---

* content
{:toc}

Git Worktrees
------------------------

## main working tree VS linked working tree
```
git worktree add <path> [<branch>]
git worktree add ../tmparea docs
git worktree add --force ../tmparea docs
git worktree add -b my-branch ../tmparea3 master
git worktree add --detach ../tmparea3 master

git worktree list
git worktree list --porcelain

git worktree prune
git worktree prune -n  # --dry-run
git worktree prune -v
```

# submodules

.gitmodules
.git/module

```
git submodule add <url to mod1> mod1
git submodule status
git submodule init
git config -l | grep submodule
git submodule update
git submodule update --init # the shortcut
git clone --recursive <URL of container project> # further
git clone --recurse-submodules <URL of container project> # same as above
```

```
git submodule [--quiet] foreach [--recursive] <command>
git submodule foreach git log --oneline
git pull --recurse-submodules
git submodule foreach git pull origin master
git diff --submodule
```
## status
```
“-“ if the submodule is not initialized
“+” if the submodule’s current version that’s checked out is different from the SHA1 in the containing repository
“U” if there are merge con icts in the submodule
```
## variable
```
$name—the name of the submodule
$path—the path of the submodule relative to the superproject
$sha1—the current SHA1 value of the submodule as recorded in the superproject 
$toplevel—the absolute path to the superproject

git submodule --quiet foreach 'echo $path $sha1'
```

# deinit

```
git submodule deinit
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
