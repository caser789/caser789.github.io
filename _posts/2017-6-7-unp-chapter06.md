---
layout: post
title: UNP Chapter 06 I/O Multiplexing: the select and poll functions
date:   2017-06-07 15:18:01 +0100
categories: c unp select poll multiplex
tag: select
---

* content
{:toc}

## intro
## I/O models

### blocking I/O
### nonblocking I/O
polling by a loop
### I/O multiplexing 
* blocking select
* when readable then recvfrom
### signal driven I/O
* sigaction
* kernel tell us when I/O can be initiated
### asynchronous I/O
* kernel tell us when I/O is complete

#### 2 phases
* waiting for date data to be ready
* copy data from kernel to process

## select function
```
#include <sys/select.h>
#include <sys/time.h>

int select(int maxfdp1, fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timeval *timeout)
```
```
struct timeval {
    long tv_sec;
    long tv_usec;
}
```

### 3 timeout
* wait forever by null pointer
* fixed amount of time
* do not wait, polling

## str_cli function
```
#include "unp.h"

void
str_cli(FILE *fp, int sockfd) {
    int maxfdp1;
    fd_set rset;
    char sendline[MAXLINE], recvline[MAXLINE];

    FD_ZERO(*rset);
    for ( ; ; ) {
        FD_SET(fileno(fp), &rset);
        FD_SET(sockfd, &rset);
        maxfdp1 = max(fileno(fp), sockfd) + 1;
        Select(maxfdp1, &rset, NULL, NULL, NULL);
        if (FD_ISSET(sockfd, &rset)) {
            if (Readline(sockfd, recvline, MAXLINE) == 0)
                err_quit("str_cli: server terminated prematurely");
            Fputs(recvline, stdout);
        }
        if (FD_ISSET(fileno(fp), &rset)) {
            if (Fgets(sendline, MAXLINE, fp) == NULL)
                return;
            Writen(sockfd, sendline, strlen(sendline));
        }
    }
}
```
## batch input and buffering

there might be data in pipe when client encounter EOF, so you cannot close socket
mixing stdio and select is considered very error-prone and should only be done with great care

## shutdown function
```
#include <sys/socket.h>
int shutdown(int sockfd, int howto);
```
* send a FIN to server, leave the socket open for reading
### close's limitation
* close related with ref count, shutdown donot
* close terminate both direction, shutdown can close half of a TCP connection

### 3 howtos
* SHUT_RD 0
* SHUT_WR 1
* SHUT_RDWR = SHUT_RD + SHUT_WR 2

## str_cli function
```
#include "unp.h"

void
str_cli(FILE *fp, int sockfd) {
    int maxfdp1, stdineof;
    fd_set rset;
    char buf[MAXLINE];
    int n;
    stdinefo = 0;
    FD_ZERO(&rset);
    for ( ; ; ) {
        if (stdineof == 0)
            FD_SET(fileno(fp), &rset);
        FD_SET(sockfd, &rset);
        maxfdp1 = max(fileno(fp), sockfd) + 1;
        Select(maxfdp1, &rset, NULL, NULL, NULL);

        if (FD_ISSET(sockfd, &rset)) {
            if ( (n = Read(sockfd, buf, MAXLINE)) == 0) {
                if (stdineof == 1)
                    return;
                else
                    err_quit("str_cli: server terminated prematurely");
            }
        }

        if (FD_ISSET(fileno(fp), &rset)) {
            if ( (n = Read(fileno(fp), buf, MAXLINE)) == 0) {
                stdinoef = 1;
                Shutdown(sockfd, SHUT_WR);
                FD_CLR(fileno(fp), &rset);
                continue;
            }
            Writen(sockfd, buf, n);
        }
    }
}
```

## TCP echo server
```
#include "unp.h"

int
main(int argc, char **argv) {
    int i, maxi, maxfd, listenfd, connfd, sockfd;
    int nready, client[FD_SETSIZE];
    ssize_t n;
    fd_set rset, allset;
    char buf[MAXLINE];
    socklen_t clilen;
    struct sockaddr_in cliaddr, servaddr;

    listenfd = Socket(AF_INET, SOCK_STREAM, 0);

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(SERV_PORT);

    Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

    Listen(listenfd, LISTENQ);

    maxfd = listenfd;
    maxi = -1;
    for (i = 0; i < FD_SETSIZE; i++)
        client[i] = -1
    FD_ZERO(&allset);
    FD_SET(listenfd, &allset);

    for ( ; ; ) {
        rset = allset;
        nready = Select(maxfd + 1, &rset, NULL, NULL, NULL);

        if (FD_ISSET(listenfd, &rset)) {
            clilen = sizeof(cliaddr);
            connfd = Accept(listenfd, (SA *) &cliaddr, &clilen);

            for (i = 0; i < FD_SETSIZE; i++)
                if (client[i] < 0) {
                    client[i] = connfd;
                    break
                }
            if (i == FD_SETSIZE)
                err_quit("too many clients");
            FD_SET(connfd, &allset);
            if (connfd > maxfd)
                maxfd = connfd;
            if (i > maxi)
                maxi = i;

            if (--nready <= 0)
                continue
        }
        for (i = 0; i <= maxi; i++) {
            if ( (sockfd = client[i]) < 0)
                continue;
            if (FD_ISSET(sockfd, &rset)) {
                if ( (n = Read(sockfd, buf, MAXLINE)) == 0) {
                    Close(sockfd);
                    FD_CLR(sockfd, &allset);
                    client[i] = -1;
                } else
                    Writen(sockfd, buf, n);
                if (--nready <= 0)
                    break;
            }
        }
    }
}
```
## pselect function
```
#include <sys/select.h>
#include <signal.h>
#include <time.h>
int pselect (int maxfdp1, fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timespec *timeout, const sigset_t *sigmask)
```

## poll function
```
#include <poll.h>
int poll(struct pollfd *fdarray, unsigned long nfds, int timeout);
```

## TCP echo server

## summary


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
