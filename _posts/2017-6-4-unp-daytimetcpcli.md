---
layout: post
title: UNP: a daytime tcp client
date:   2017-06-04 15:18:01 +0100
categories: c unp
tag: unp
---

* content
{:toc}

## code

```c
# include "unp.h"

int
main(int argc, char **argv) {
    int sockfd, n;
    char recvline[MAXLINE + 1];
    struct sockaddr_in servaddr;

    if (argc != 2)
        err_quit("usage: a.out <IPaddress>");

    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0)
        err_sys("socket error");

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(13); /* daytime server */
    if (inet_pton(AF_INET, argv[1], &servaddr.sin_addr) <= 0)
        err_quit("inet_pton error for %s", argv[1]);

    if (connect(sockfd, (SA *) &servaddr, sizeof(servaddr)) < 0)
        err_sys("connect error");

    while ((n = read(sockfd, recvline, MAXLINE)) > 0) {
        recvline[n] = 0; /* null terminate */
        if (fputs(recvline, stdout) == EOF)
            err_sys("fputs error");
    }
    if (n < 0)
        err_sys("read error");

    exit(0);
}
```

## exp

### assign and compare

```c
sockfd = socket(AF_INET, SOCK_STREAM, 0);
if (sockfd < 0)
```

```c
if ( (sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0)
    err_sys("socket error");
```

### bzero

* memset bugs

### htons

* host to network short
* convert the binary port number into the proper format

### inet_pton

* presentation to numeric
* convert 206.62.226.35 into the proper format


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
