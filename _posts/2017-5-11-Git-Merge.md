---
layout: post
title: Git Merge
date:   2017-05-11 11:54:01 +0800
categories: git merge
tag: git
---

* content
{:toc}

Git Merge
------------------------

# view diffs when having merge conflict

`git log --merge -p file1.txt`

# Strategies
1. Resolve strategy
2. Recursive strategy
    
    * -X Ours
    * -X Theirs
    * -X ignore-space-change
    * -X ignore-all-space
    * -X ignore-space-at-eol

    ```
    git cherry-pick -Xtheirs d9e3b4
    ```

3. Octopus strategy
4. Ours strategy

# Set merge conflict to show ancestor

```
git config --global merge.conflictstyle diff3
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
