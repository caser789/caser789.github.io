---
layout: post
title: UNP: a daytime tcp client
date:   2017-06-04 15:18:01 +0100
categories: c unp
tag: unp
---

* content
{:toc}

## v4

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
## v6
```c
# include "unp.h"

int
main(int argc, char **argv) {
    int sockfd, n;
    char recvline[MAXLINE + 1];
    struct sockaddr_in6 servaddr;

    if (argc != 2)
        err_quit("usage: a.out <IPaddress>");

    if ((sockfd = socket(AF_INET6, SOCK_STREAM, 0)) < 0)
        err_sys("socket error");

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin6_family = AF_INET6;
    servaddr.sin6_port = htons(13); /* daytime server */
    if (inet_pton(AF_INET6, argv[1], &servaddr.sin6_addr) <= 0)
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

## socket wrapper
```c
int
Socket(int family, int type, int protocol) {
    int n;
    if ( (n = socket(family, type, protocol)) < 0)
        err_sys("socket error")
    return (n);
}
```
## error num wrapper

> thread functions do not set the standard Unix errno variable when an error occurs; instead, the errno value is the return value of the function.

```c
int n;
if ( (n = pthread_mutex_lock(&ndone_mutex)) != 0)
    errno = n, err_sys("pthread_mutex_lock error");
```
```c
void
Pthread_mutex_lock(pthread_mutex_t *mptr) {
    int n;
    if ( (n = pthread_mutex_lock(mptr)) == 0)
        return;
    errno = n;
    err_sys("pthread_mutex_lock error");
}

Pthread_mutex_lock(&ndone_mutex);
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

## server
```c
# include "unp.h"
# include <time.h>

int
main(int argc, char **argv) {
    int listenfd, connfd;
    struct socketaddr_in servaddr;
    char buff[MAXLINE];
    time_t ticks;

    listenfd = Socket(AF_INET, SOCK_STREAM, 0);
    bzeros(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(13);

    Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

    Listen(listenfd, LISTENQ);

    for ( ; ; ) {
        connfd = Accept(listenfd, (SA *) NULL, NULL);
        ticks = time(NULL);
        snprintf(buff, sizeof(buff), "%.24s\r\n", ctime(&ticks));
        Write(connfd, buff, strlen(buff));
        Close(connfd);
    }
}
```
### infinite loop

```c
for ( ; ; ) {
}
```

### time

returns the number of seconds since the Unix Epoch: 00:00:00 January 1, 1970, Coordinated Universal Time (UTC)

### ctime

converts this integer value into a human-readable string such as
`Mon May 26 20:58:40 2003`

### snprintf

* `A carriage return and linefeed are appended to the string by snprintf`
* check buf not overflow
* dangerouse: `gets`, `strcat`, `strcpy`, use `fgets`, `strncat`, `strncpy` instead, even better `strlcat`, `strlcpy`

### server
* iterative server
* concurrent server

### netstat

netstat -ni
netstat -nr  show route table
### ifconfig
ifconfig eth0



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
