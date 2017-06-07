---
layout: post
title: UNP Chapter 04 Elementary TCP Sockets
date:   2017-06-06 08:18:01 +0100
categories: c unp socket tcp
tag: socket
---

* content
{:toc}


## Intro
## socket function

```
#include <sys/socket.h>
int socket (int family, int type, int protocol);
```

family   | Description
-----    | -----
AF_INET  | IPv4 protocols
AF_INET6 | IPv6 protocols
AF_LOCAL | Unix domain protocols
AF_ROUTE | Routing sockets
AF_KEY   | Key socket

type           | Description
-----          | -----
SOCK_STREAM    | stream socket
SOCK_DGRAM     | datagram socket
SOCK_SEQPACKET | sequenced packet socket
SOCK_RAW       | raw socket

Protocol     | Description
-----        | -----
IPPROTO_TCP  | TCP
IPPROTO_UDP  | UDP
IPPROTO_SCTP | SCTP

### AF_XXX vs PF_XXX
AF for address family
PF for protocol family

## connect function
```
#include <sys/socket.h>
int connect(int sockfd, const struct sockaddr *servaddr, socklen_t addrlen);
```
### errors
* EHOSTUNREACH
* RST
* ETIMEOUT

## bind function
```
#include <sys/socket.h>
int bind(int sockfd, const struct sockaddr *myaddr, socklen_t addrlen);
```

```
struct sockaddr_in serveraddr;
servaddr.sin_addr.s_addr = htonl (INADDR_ANY);  /* wildcard */
```

```
struct sockaddr_in6 serv;
serv.sin6_addr = in6addr_any; /* wildcard */
```

### errors
* EADDRINUSE

## listen function
socket from CLOSED --> LISTEN

```
#include <sys/socket.h>
#int listen (int sockfd, int backlog);
```

### 2 queues in kernel

* incomplete connection queue, SYN_RCVD state
* completed connection queue, ESTABLISHED state

### backlog

specified the maximum value for the sum of both queues
### listen wrapper
```
void
Listen (int fd, int backlog) {
    char *ptr;
    if ( (ptr = getenv("LISTENQ")) != NULL)
        backlog = atoi (ptr);
    if (listen (fd, backlog) < 0)
        err_sys ("listen error");
}
```
## accept function
```
#include <sys/socket.h>
int accept (int sockfd, struct sockaddr *cliaddr, socklen_t *addrlen);
```
addrlen is a value-result

listening socket
connected socket

### daytime server which prints client ip and port
```
#include "unp.h"
#include <time.h>

int main(int argc, char **argv) {
    int listenfd, connfd;
    socklen_t len;
    struct sockaddr_in servaddr, cliaddr;
    char buff[MAXLINE];
    time_t ticks;

    listenfd = Socket(AF_INET, SOCK_STREAM, 0);

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htonl(13);

    Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

    Listen(listenfd, LISTENQ);

    for ( ; ; ) {
        len = sizeof(cliaddr);
        connfd = Accept(listenfd, (SA *) &cliaddr, &len);
        printf("connection from %s, port %d\n", Inet_ntop(AF_INET, &cliaddr.sin_addr, buff, sizeof(buff)), ntohs(cliaddr.sin_port));
        ticks = time(NULL);
        snprintf(buff, sizeof(buff), "%.24s\r\n", ctime(&ticks));
        Write(connfd, buff, strlen(buff));
        Close(connfd);
    }
}
```
## fork and exec function
```
#include <unistd.h>
pid_t fork(void);
```
child read write connected socket, parent close it

### 6 exec functions
```
int execl (const char *pathname, const char *arg0, ...);
int execv (const char *pathname, char *const argv[]);
int execle (const char *pathname, const char *arg0, ... 0, char *cosnt envp[]);
int execve (const char *pathname, char *const, argv[], char *const envp[]);
int execlp (const char *filename, const char *arg0, ...);
int execvp (const char *filename, char *const argv[]);
```
## concurrent servers
```
pid_t pid;
int listenfd, connfd;

listenfd = Socket();
Bind(listenfd, ...);
Listen(listenfd, LISTENQ);
for ( ; ; ) {
    connfd = Accept(listenfd, ...);
    if ( (pid = Fork()) == 0) {
        Close(listenfd);
        doit(connfd);
        Close(connfd);
        exit(0);
    }
    Close(connfd);
}


```

### ref count and close of connection
## close function
```
#include <unistd.h>
int close (int sockfd);
```
### descriptor ref count
### shutdown function to send FIN

## getsockname and getpeername function
```
#include <sys/socket.h>
int getsockname (int sockfd, struct sockaddr *localaddr, socklen_t *addrlen);
int getpeername (int sockfd, struct sockaddr *peeraddr, socklen_t *addrlen);

```
addrlen is a value-result

### obtain AF of a socket
```
#include "unp.h"
int
sockfd_to_family (int sockfd) {
    struct sockaddr_storage ss;
    socklen_t len;
    len = sizeof(ss);
    if (getsockname(sockfd, (SA *) &ss, &len) < 0)
        return (-1);
    return (ss.ss_family);
}
```
## Summary

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
