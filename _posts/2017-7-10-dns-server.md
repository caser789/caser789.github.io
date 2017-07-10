---
layout: post
title: Deploy DNS server
date:   2017-07-10 09:31:01 +0800
categories: dns server deploy
tag: dns
---

* content
{:toc}


## install bind

```
sudo apt-get update
sudo apt-get install bind9 bind9utils bind9-doc
```

## set bind to IPv4 Mode

`sudo vim /etc/default/bind9`

```
添加 “-4” 参数

OPTIONS="-4 -u bind"
```

## 配置 Options file，添加全局配置

`sudo vim /etc/bind/named.conf.options`

在 options 节点中，添加如下配置

```
auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
        recursion yes;                 # enables resursive queries
        allow-query { any; };
        listen-on { 192.168.0.53;54.222.160.71; };
        allow-transfer { none; };      # disable zone transfers by default

        forwarders {
            8.8.8.8;
            8.8.4.4;
        };
```

## 配置Local file（在此文件中，添加新域名）

`sudo vim /etc/bind/named.conf.local`

```
zone "surpax.net" {
    type master;
    file "/etc/bind/db.surpax.net";
};
```
## 添加zone文件，配置Resource Record

`sudo vim /etc/bind/db.surpax.net`

```
; Zone file for surpax.net
$TTL    3600
@       IN      SOA     ns1.surpax.net.    root.surpax.net. (
                     2016070716         ; Serial
                           3600         ; Refresh
                           1800         ; Retry
                         604800         ; Expire
                          43200 )       ; Negative Cache TTL
        IN      NS      ns1.surpax.net.
@       IN      A       54.86.40.163
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
