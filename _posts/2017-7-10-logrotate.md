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

## set file size

```
minsize size

    after time interval and size, log will be rotated

size size (100, 100k, 100M, 100G)

    mutally exclusive with time interval option

maxsize size
    
    as soon as log file grow bigger than size

```

## config rotate time

### crontab

/etc/crontab, run every 5 minutes

```
*/5 * * * * /etc/cron.daily/logrotate
```

### cron.daily

moving the script /etc/cron.daily/logrotate to /etc/cron.hourly/logrotate


## refs

https://stackoverflow.com/questions/20162176/centos-linux-setting-logrotate-to-maximum-file-size-for-all-logs

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
