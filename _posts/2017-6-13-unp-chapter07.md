---
layout: post
title: UNP Chapter 07 Socket Options
date:   2017-06-13 03:18:01 +0100
categories: c unp socket
tag: socket
---

* content
{:toc}

## Intro
* getsockopt
* setsockopt
* fcntl
* ioctl
## getsockopt and setsockopt functions
```
# include <sys/socket.h>

int getsockopt(int sockfd, int level, int optname, void *optval, socklen_t *optlen);
int setsockopt(int sockfd, int level, int optname, const void *optval socklen_t optlen);
```

### level
* the general socket code
* protocol-specific code

## checking if an option is supported and obtaining the default
`sockopt/checkopts.c`
```
#include "unp.h"
#include <netinet/tcp.h>
union val {
    int i_val;
    long l_val;
    struct linger linger_val;
    struct timeval timeval_val;
} val;
static char *sock_str_flag(union val *, int);
static char *sock_str_int(union val *, int);
static char *sock_str_linger(union val *, int);
static char *sock_str_timeval(union val *, int);
struct sock_opts {
    const char *opt_str;
    int opt_level;
    int opt_name;
    char *(*opt_val_str) (union val *, int);
} sock_opts[] = {
    {"SO_BROADCAST", SOL_SOCKET, SO_BROADCAST, sock_str_flag},
    {"SO_DEBUG", SOL_SOCKET, SO_DEBUG, sock_str_flag},
    {"SO_DONTROUTE", SOL_SOCKET, SO_DONTROUTE, sock_str_flag},
    {"SO_ERROR", SOL_SOCKET, SO_ERROR, sock_str_int},
    {"SO_KEEPALIVE", SOL_SOCKET, SO_KEEPALIVE, sock_str_flag},
    {"SO_LINGER", SOL_SOCKET, SO_LINGER, sock_str_linger},
    {"SO_OOBINLINE", SOL_SOCKET, SO_OOBINLINE, sock_str_flag},
    {"SO_RCVBUF", SOL_SOCKET, SO_RCVBUG, sock_str_int},
    {"SO_SNDBUF", SOL_SOCKET, SO_SNDBUF, sock_str_int},
    {"SO_RCVLOWAT", SOL_SOCKET, SO_RCVLOWAT, sock_str_int},
    {"SO_SNDLOWAT", SOL_SOCKET, SO_SNDLOWAT, sock_str_int},
    {"SO_RCVTIMEO", SOL_SOCKET, SO_RCVTIMEO, sock_str_timeval},
    {"SO_SNDTIMEO", SOL_SOCKET, SO_SNDTIMEO, sock_str_timeval},
    {"SO_REUSEADDR", SOL_SOCKET, SO_REUSEADDR, sock_str_flag},
    {"SO_REUSEPORT", SOL_SOCKET, SO_REUSEPORT, sock_str_flag},
    {"SO_TYPE", SOL_SOCKET, SO_TYPE, sock_str_int},
    {"SO_USELOOPBACK", SOL_SOCKET, SO_USELOOPBACK, sock_str_flag},
    {"IP_TOS", IPPROTO_IP, IP_TOS, sock_str_int},
    {"IP_TTL", IPPROTO_IP, IP_TTL, sock_str_int},
    {"IPV6_DONTFRAG", IPPROTO_IPV6, IPV6_DONTFRAG, sock_str_flag},
    {"IPV6_UNICAST_HOPS", IPPROTO_IPV6, IPV6_UNICAST_HOPS, sock_str_int},
    {"IPV6_V6ONLY", IPPROTO_IPV6, IPV6_V6ONLY, sock_str_flag},
    {"TCP_MAXSEG", IPPROTO_TCP, TCP_MAXSEG, sock_str_int},
    {"TCP_NODELAY", IPPROTO_TCP, TCP_NODELAY, sock_str_flag},
    {"SCTP_AUTOCLOSE", IPPROTO_SCTP, SCTP_AUTOCLOSE, sock_str_int},
    {"SCTP_MAXBURST", IPPROTO_SCTP, SCTP_MAXBURST, sock_str_int},
    {"SCTP_MAXSEG", IPPROTO_SCTP, SCTP_MAXSEG, sock_str_int},
    {"SCTP_NODELAY", IPPROTO_SCTP, SCTP_NODELAY, sock_str_int},
    {NULL, 0, 0, NULL},
}

int
main(int argc, char **argv) {
    int fd;
    socklen_t len;
    struct sock_opts *ptr;
    for (ptr = sock_opts; ptr->opt_str != NULL; ptr++) {
        printf("%s: ", ptr->opt_str);
        if (ptr->opt_val_str == NULL)
            printf("(undefined)\n");
        else {
            switch (ptr->opt_level) {
                case SOL_SOCKET:
                case IPPROTO_IP:
                case IPPROTO_TCP:
                    fd = Socket(AF_INET, SOCK_STREAM, 0);
                    break;
                case IPPROTO_IPV6:
                    fd = Socket(AF_INET6, SOCK_STREAM, 0);
                    break;
                case IPPROTO_SCTP:
                    fd = Socket(AF_INET, SOCK_SEQPACKET, IPPROTO_SCTP);
                    break;
                default:
                    err_quit("Can't create fd for level %d\n", ptr->opt_level);
            }
            len = sizeof(val);
            if (getsockopt(fd, ptr->opt_level, ptr->opt_name, &val, &len) == -1)
                err_ret("getsocketopt error");
            else
                printf("default = %s\n", (*ptr->opt_val_str) (&val, len));
            close(fd);
        }
    }
    exit(0);
}
```
## socket states
SO_DEBUG
SO_DONTROUTE
SO_KEEPALIVE
SO_LINGER
SO_OOBINLINE
SO_RCVBUF
SO_RCVLOWAT
SO_SNDBUF
SO_SNDLOWAT
TCP_MAXSEG
TCP_NODELAY
## generic socket options, protocol independent
SO_BROADCAST
SO_DEBUG
SO_DONTROUTE
SO_ERROR
SO_KEEPALIVE
SO_LINGER
## IPv4 socket options
## ICMPv6 socket options
## IPv6 socket options
## TCP socket options
## SCTP socket options
## fcntl function
* file control
## summary


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
