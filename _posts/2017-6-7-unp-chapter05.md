---
layout: post
title: UNP Chapter 05 TCP Server/Client example
date:   2017-06-07 01:18:01 +0100
categories: c unp tcp server client
tag: tcp
---

* content
{:toc}


## Intro

## TCP Echo server: main function
### create socket, bind server's well-known port
socket()
bind()
listen()
### wait for client connection to complete
accept()

### example
```
#include "unp.h"
int
main(int argc, char **argv) {
    int listenfd, connfd;
    pid_t childpid;
    socklen_t clilen;
    struct sockaddr_in cliaddr, servaddr;

    listenfd = Socket(AF_INET, SOCK_STREAM, 0);

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(SERV_PORT);

    Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

    Listen(listenfd, LISTENQ);

    for ( ; ; ) {
        clilen = sizeof(cliaddr);
        connfd = Accept(listenfd, (SA *) $cliaddr, &clilen);
        if ( (childpid = Fork()) == 0) {
            Close(listenfd);
            str_echo(connfd);
            exit (0);
        }
        Close(connfd);
    }
}
```

## TCP Echo server: str_echo function
```
#include "unp.h"

void
str_echo(int sockfd) {
    ssize_t n;
    char buf[MAXLINE];
    again:
        while ( (n = read(sockfd, buf, MAXLINE)) > 0)
            Writen(sockfd, buf, n);
        if (n < 0 && errno == EINTR)
            goto again;
        else if (n < 0)
            err_sys("str_echo: read error");
}
```
## TCP Echo client: main function
```
#include "unp.h"
int
main(int argc, char **argv) {
    int sockfd;
    struct sockaddr_in servaddr;

    if (argc != 2)
        err_quit("usage: tcpcli <IPaddress>");

    sockfd = Socket(AF_INET, SOCK_STREAM, 0);

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(SERV_PORT);
    Inet_pton(AF_INET, argv[1], &servaddr.sin_addr);

    Connect(sockfd, (SA *) &servaddr, sizeof(servaddr));

    str_cli(stdin, sockfd);
    exit(0);
}
```
## TCP Echo client: str_cli function
```
#include "unp.h"

void
str_cli(FILE *fp, int sockfd) {
    char sendline[MAXLINE], recvline[MAXLINE];
    while (Fgets(sendline, MAXLINE, fp) != NULL) {
        Writen(sockfd, sendline, strlen (sendline));
        if (Readline(sockfd, recvline, MAXLINE) == 0)
            err_quit("str_cli: server terminated prematurely");
        Fputs(recvline, stdout);
    }
}
```

* fgets reads a line of text
* writen sends the line to the server
* readline reads the line from server
* fputs write it to stdout

## Normal startup
```
netstat -a  # listening sockes
```

ps

```
read_chan: process is blocked on terminal I/O
```

child process becomes a zombie, you should clean it

## Normal termination

## POSIX signal handling

also called software interrupts

process --> process
kernel --> process

### signal
1. disposition: action associated
2. sigaction()

### actions
1. signal handler (catching a signal), SIGKILL, SIGSTOP cannot be catched
2. ignore SIG_IGN    SIGKILL, SIGSTOP cannot be ignored
3. default SIG_DFL

### signal function
```
@include "unp.h"

Sigfunc *
signal (int signo, Sigfunc *func) {
    struct sigaction act, oact;

    act.sa_handler = func;
    sigemptyset (&act.sa_mask);
    act.sa_flags = 0;
    if (signo == SIGALRM) {
        act.sa_flats |= SA_INTERRUPT;
    } else {
        act.sa_flats |= SA_RESTART;
    }
    if (sigaction (signo, &act, &oact) < 0)
        return (SIG_ERR);
    return (oact.sa_handler);
}
```

## Handling SIGCHLD signals

### handling zombies
```
Signal (SIGCHLD, sig_chld);
```

```
#include "unp.h"

void
sig_chld (int signo) {
    pid_t pid;
    int stat;
    pid = wait(&stat);
    printf("child %d terminated\", pid);
    return;
}
```

```
for ( ; ; ) {
    clilen = sizeof(cliaddr);
    if ( (connfd = accept(listenfd, (SA *) &cliaddr, &clilen)) < 0) {
        if (errno = EINTR)
            continue
        else
            err_sys("accept error");
    }
}
```
## wait and waitpid functions

```
#include <sys/wait.h>

pid_t wait(int *statloc);
pid_t waitpid(pid_t pid, int *statloc, int options);
```

```
#include "unp.h"

int
main (int argc, char **argv) {
    int i, sockfd[5];
    struct sockaddr_in servaddr;

    if (argc != 2)
        err_quit("usage: tcpcli <IPaddress>");

    for (i = 0; i < 5; i++) {
        sockfd[i] = Socket(AF_INET, SOCK_STREAM, 0);

        bzero(&servaddr, sizeof(servaddr));
        aservaddr.sin_family = AF_INET;
        servaddr.sin_port = htons(SERV_PORT);
        Inet_pton (AF_INET, argv[1], &servaddr.sin_addr);

        Connect(sockfd[i], (SA *) &servaddr, sizeof(servaddr));
    }

    str_cli (stdin, sockfd[0]);
    exit(0);
}
```

```
#include "unp.h"

void
sig_chld (int signo) {
    pid_t pid;
    int stat;

    while ( (pid = waitpid(-1, &stat, WNOHANG)) > 0)
        printf("child %d terminated\n", pid);
    return;
}
```

```
#include "unp.h"
int
main (int argc, char **argv) {
    int listenfd, connfd;
    pid_t childpid;
    socklen_t clilen;
    struct sockaddr_in cliaddr, servaddr;
    void sig_chld(int);

    listenfd = Socket (AF_INET, SOCK_STREAM, 0);

    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(SERV_PORT);

    Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));
    Listen(listenfd, LISTENQ);
    Signal(SIGCHLD, sig_chld);

    for ( ; ; ) {
        clilen = sizeof(cliaddr);
        if ( (connfd = accept(listenfd, (SA *) &cliaddr, &clilen)) < 0) {
            if (errno == EINTR)
                continue
            else
                err_sys("accept error");
        }
        if ( (childpid = Fork()) == 0) {
            Close(listenfd);
            str_echo(connfd);
            exit(0);
        }
        Close(connfd);
    }
}
```
## connection abort before accept returns

## termination of server process

## SIGPIPE signal

## crashing of server host

## crashing and rebooting of server host

## shutdown of server host

## summary of TCP example

## Data format
### test string
```
#include "unp.h"

void str_echo (int sockfd) {
    long arg1, arg2;
    ssize_t n;
    char line[MAXLINE];

    for ( ; ; ) {
        if ( (n = Readline(sockfd, line, MAXLINE)) == 0)
            return;

        if (sscanf(line, "%ld%ld", &arg1, &arg2) == 2)
            snprintf(line, sizeof(line), "%ld\n", arg1 + arg2);
        else
            snprintf(line, sizeof(line), "input error\n");

        n = strlen(line);
        Writen(sockfd, line, n);
    }

}
```
### binary

```
struct args {
    long arg1;
    long arg2;
}
struct result {
    long sum;
}
```

```
#include "unp.h"
#include "sum.h"

void
str_cli(FILE *fp, int sockfd) {
    char sendline[MAXLINE];
    struct args args;
    struct result result;
    while (Fgets(sendline, MAXLINE, fp) != NULL) {
        if (sscanf(sendline, "%ld%ld", &args.arg1, &args.arg2) != 2) {
            printf("invalid input: %s", sendline);
            continue;
        }
        Writen(sockfd, &args, sizeof(args));
        if (Readn(sockfd, &result, sizeof(result)) == 0)
            err_quit("str_cli: server terminated prematurely");
        printf("%ld\n", result.sum);
    }
}
```

## summary

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
