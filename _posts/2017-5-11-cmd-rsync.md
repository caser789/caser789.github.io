---
layout: post
title: rsync
date:   2017-05-11 09:54:01 +0800
categories: cmd rsync
tag: cmd
---

* content
{:toc}

Rsync
------------------------

## Sync remote dir with local dir via ssh

```
rsync -e ssh --progress -r alertservcie/ alertservcie_server:/console/www
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
