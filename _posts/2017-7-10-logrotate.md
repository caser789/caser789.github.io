---
layout: post
title: Logrotate config
date:   2017-07-10 05:18:01 +0800
categories: linux ubuntu logrotate
tag: logrotate
---

* content
{:toc}

## running status

check last rotate time

`cat /var/lib/logrotate/status`

## debug mode

`logrotate -v -d /etc/logrotate.conf`

## example config

```
/var/logs/* {
    rotate 3
    daily                 # 每天rotate
    missingok
    notifempty
    compress        # 压缩
    delaycompress
    sharedscripts
    postrotate
        reload rsyslog >/dev/null 2>&1 || true
    endscript
}
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
