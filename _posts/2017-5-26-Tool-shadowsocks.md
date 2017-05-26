---
layout: post
title: shadowsocks
date:   2017-05-26 11:18:01 +0100
categories: shadowsocks vpn
tag: shadowsocks
---

* content
{:toc}

## Install

```
sudo apt-get install python-pip python-m2crypto
sudo pip install shadowsocks
```

## Create a configuration file

`/etc/shadowsocks.json`

```
{
  "server":"your_server_ip",
  "server_port":8000,
  "local_port":1080,
  "password":"your_passwd",
  "timeout":600,
  "method":"aes-256-cfb"
}
```
## start

`sudo ssserver -c /etc/shadowsocks.json -d start`

## stop

`sudo ssserver -d stop`

## restart

`sudo ssserver -c /etc/shadowsocks.json -d restart`

## log

`less /var/log/shadowsocks.log`

## on startup

`/etc/rc.local`

```
/usr/bin/python /usr/local/bin/ssserver -c /etc/shadowsocks.json -d start
```

refs

[ubuntu](https://www.linuxbabe.com/linux-server/setup-your-own-shadowsocks-server-on-debian-ubuntu-centos)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
