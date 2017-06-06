---
layout: post
title: UNP Chapter 03 Sockets introduction
date:   2017-06-05 16:18:01 +0100
categories: c unp socket
tag: socket
---

* content
{:toc}

## Introduction
## Socket address structure
### IPv4 socket address structure
also named `Internet socket address structure`
named `sockaddr_in`
netinet/in.h
```
struct in_addr {
    in_addr_t s_addr
};

struct sockaddr_in {
    uint8_t sin_len;
    sa_family_t sin_family;
    in_port_t sin_port;
    struct in_addr sin_addr;
    char sin_zero[8];
}
```
### generic socket address structure
sys/socket.h
```
struct sockaddr {
    uint8_t sa_len;
    sa_family_t sa_family;
    char sa_data[14];
};
```

```
int bind(int, struct sockaddr *, socklen_t)
```

```
struct sockaddr_in serv;
/* fill in serv{} */
bind(sockfd, (struct sockaddr *) &serv, sizeof(serv));
```
### IPv6 socket address structure

netinet/in.h
```
struct in6_addr {
    uint8_t s6_addr[16];
}
# define SIN6_LEN
struct sockaddr_in6 {
    uint8_t sin6_len;
    sa_family_t sin6_family;
    in_port_t sin6_port;
    uint32_t sin6_flowinfo;
    struct in6_addr sin6_addr;
    uint32_t sin6_scope_id;
};
```
### new generic socket address structure
netinet/in.h
```
struct sockaddr_storage {
  uint8_t      ss_len;       /* length of this struct (implementation dependent) */
  sa_family_t  ss_family;    /* address family: AF_xxx value */
};
```
### comparison of socket address structures

## Value-result arguments

copy data from process to kernel

```
struct sockaddr_in serv;
/* fill in serv{} */
connect (sockfd, (SA *) &serv, sizeof(serv));
bind (sockfd, (SA *) &serv, sizeof(serv));
sendto (sockfd, (SA *) &serv, sizeof(serv));
```


kernal to process
```
struct sockaddr_un cli;
socklen_t len;

len = sizeof(cli);
getpeername(unixfd, (SA *) &cli, &len);

accept(unixfd, (SA *) &cli, &len);
recvfrom(unixfd, (SA *) &cli, &len);
getsockname(unixfd, (SA *) &cli, &len);
```

len is a value when function is called and a result when the function returns

### other value-result argument

* the middle three arguments for the select function
* the length argument for the getsockopt function
* the msg_namelen and msg_controllen members of the msghdr structure, when used with recvmsg
* the ifc_len member of the ifconf structure
* the first of the 2 length arguments for the sysctl function

## Byte Ordering Functions

### 2 type of the host byte order
* little-endian
* big-endian
### a program to determine host byte order
```
#include "unp.h"

int
main(int argc, char **argv) {
    union {
        short s;
        char c[sizeof(short)];
    } un;

    un.s = 0x0102;
    printf("%s: ", CPU_VENDOR_OS);
    if (sizeof(short) == 2) {
        if (un.c[0] == 1 && un.c[1] == 2) {
            printf("big-endian\n");
        } else if (un.c[0] == 2 && un.c[1] ==1)
            printf("little-endian\n");
        else
            printf("unknown\n");
    } else
        printf("sizeof(short) = %d\n", sizeof(short));

    exit(0);
}
```
### convert between host byte order and network byte order
```
#include <netinet/in.h>

uint16_t htons(uint16_t host16bitvalue);
uint32_t htonl(uint32_t host32bitvalue);
uint16_t ntohs(uint16_t net16bitvalue);
uint32_t ntohl(uint32_t net32bitvalue);
```

## Byte manipulation Functions
```
#include <string.h>

void bzero(void *dest, size_t nbytes);
void bcopy(const void *src, void *dest, size_t nbytes);
int bcmp(const void *ptr1, const void *ptr2, size_t nbytes);
```
### const
the memory pointed by the const pointer is read but not motified by the function

### bzero
* set the specified number of bytes to 0 in the dest
### bcopy
* move bytes from source to dest
### bcmp
compare 2 byte string, 0: identical, otherwise nonzero

```
#include <string.h>
void *memset(void *dest, int c, size_t len);
void *memcpy(void *dest, const void *src, size_t nbytes);
int memcmp(const void *ptr1, const void *ptr2, size_t nbytes);
```
## inet_aton, inet_addr, inet_ntoa Functions
convert IPv4 address from dotted-decimal string ("192.0.0.1") to 32-bit network byte ordered binary value
```
#include <arpa/inet.h>
int inet_aton(const char *strptr, struct in_addr *addrptr);
in_addr_t inet_addr(const char *strptr);
char *inet_ntoa(struct in_addr inaddr);
```
## inet_pton, inet_ntop Functions
p for presentation
n for numeric

```
#include <arpa/inet.h>
int inet_pton(int family, const char *strptr, void *addrptr);
const char *inet_ntop(int family, const void *addrptr, char *strptr, size_t len);

#define INET_ADDRSTRLEN 16
#define INET6_ADDRSTRLEN 46
```
### example
```
foo.sin_addr.s_addr = inet_addr(cp);  /* deprecated */
inet_pton(AF_INET, cp, &foo.sin_addr);

ptr = inet_ntoa(foo.sin_addr);  /* deprecated */
char str[INET_ADDRSTRLEN];
ptr = inet_ntop(AF_INET, &foo.sin_addr, str, sizeof(str));
```
### inet_pton ipv4 version
```
int
inet_pton(int family, const char *strptr, void *addrptr) {
    if (family == AF_INET) {
        struct int_addr in_val;
        if (inet_aton(strptr, &in_val)) {
            memcpy(addrptr, &in_val, sizeof(struct in_addr));
            return (1);
        }
        return (0);
    }
    errno = EAFNOSUPPORT;
    return (-1);
}
```

### simple inet_ntop for ipv4
```
const char *
inet_ntop(int faimly, const void *addrptr, char *strptr, size_t len) {
    const u_char *p = (const u_char *) addrptr;

    if (family == AF_INET) {
        char temp[INET_ADDRSTRLEN];
        snprintf(temp, sizeof(temp), "%d.%d.%d.%d", p[0], p[1], p[2], p[3]);
        if (strlen(temp) >= len) {
            errno = ENOSPC;
            return (NULL);
        }
        strcpy(strptr, temp);
        return (strptr);
    }
    errno = EAFNOSUPPORT;
    return (NULL);
}
```
## sock_ntop and related Functions

to use inet_ntop
```
struct sockaddr_in addr;
inet_ntop(AF_INET, &addr.sin_addr, str, sizeof(str));
```
```
struct sockaddr_in6 addr6;
inet_ntop(AF_INET6, &addr6.sin6_addr, str, sizeof(str))
```

to be protocol-independant
```
#include "unp.h"
char *sock_ntop(const struct sockaddr *sockaddr, socklen_t addrlen);
```

```
char *
sock_ntop(const struct sockaddr *sa, socklen_t salen) {
    char portstr[8];
    static char str[128];

    switch (sa->sa_family) {
        case AF_INET: {
            struct sockaddr_in *sin = (struct sockaddr_in *) sa;
            if (inet_ntop(AF_INET, &sin->sin_addr, str, sizeof(str)) == NULL)
                return (NULL);
            if (ntohs(sin->sin_port) != 0) {
                snprintf(portstr, sizeof(portstr), ":%d", ntohs(sin->sin_port));
                strcat(str, portstr);
            }
            return (str);
        }
    }

}
```

```
#include "unp.h"
int sock_bind_wild(int sockfd, int family);
int sock_cmp_addr(const struct sockaddr *sockaddr1, const struct sockaddr *sockaddr2, socklen_t addrlen);
int sock_cmp_port(const struct sockaddr *sockaddr1, const struct sockaddr *sockaddr2, socklen_t addrlen);
int sock_get_port(const struct sockaddr *sockaddr, socklen_t addrlen);
char *sock_ntop_host(const struct sockaddr *sockaddr, socklen_t addrlen);
void sock_set_addr(struct sockaddr *sockaddr, socklen_t addrlen, void *ptr);
void sock_set_port(struct sockaddr *sockaddr, socklen_t addrlen, int port);
void sock_set_wild(struct sockaddr *sockaddr, socklen_t addrlen);
```

## readn, writen, readline Functions
```
#include "unp.h"
ssize_t readn(int filedes, void *buff, size_t nbytes);
ssize_t writen(int filedes, const void *buff, size_t nbytes);
ssize_t readline(int fildes, void *buff, size_t maxlen);
```

```
#include "unp.h"

ssize_t
readn(int fd, void *vptr, size_t n) {
    size_t nleft;
    ssize_t nread;
    char *ptr;
    ptr = vptr;
    nleft = n;
    while (nleft > 0) {
        if ( (nread = read(fd, ptr, nleft)) < 0) {
            if (errno = EINTR)
                nread = 0;
            else
                return (-1);
        } else if (nread == 0)
            break;
        nleft -= nread;
        ptr += nread;
    }
    return (n - nleft);
}
```

```
#include "unp.h"
ssize_t
writen(int fd, const void *vptr, size_t n) {
    size_t nleft;
    ssize_t nwritten;
    const char *ptr;
    ptr = vptr;
    nleft = n;
    while (nleft > 0) {
        if ( (nwritten = write(fd, ptr, nleft)) <= 0) {
            if (nwritten < 0 && errno == EINTR)
                nwritten = 0;
            else:
                return (-1)
        }
        nleft -= nwritten;
        ptr += nwritten;
    }
    return (n);
}
```

```
#include "unp.h"
ssize_t
readline(int fd, void *vptr, size_t maxlen) {
    ssize_t n, rc;
    char c, *ptr;
    ptr = vptr;
    for (n = 1, n < maxlen; n++) {
        again:
            if ( (rc = read(fd, &c, 1)) == 1) {
                *ptr++ = c;
                if (c == '\n')
                    break;
            } else if (rc == 0) {
                *ptr = 0;
                return (n - 1);
            } else {
                if (errno == EINTR)
                    goto again;
                return (-1);
            }
    }
    *ptr = 0;
    return (n);
}
```
better readline
```
#include "unp.h"

static int read_cnt;
static char *read_ptr;
static char read_buf[MAXLINE];

static ssize_t
my_read(int fd, char *ptr) {
    if (read_cnt <= 0) {
        again:
            if ( (read_cnt = read(fd, read_buf, sizeof(read_buf))) < 0) {
                if (errno == EINTR)
                    goto again;
                return (-1);
            } else if (read_cnt == 0)
                return (0);
            read_ptr = read_buf;
    }
    read_cnt--;
    *ptr = *read_ptr++;
    return (1);
}

ssize_t
readline(int fd, void *vptr, size_t maxlen) {
    ssize_t n, rc;
    char c, *ptr;
    ptr = vptr;
    for (n = 1; n < maxlen; n++) {
        if ( (rc = my_read(fd, &c)) == 1) {
            *ptr++ = c;
            if (c == '\n')
                break;
        } else if (rc == 0) {
            *ptr = 0;
            return (n - 1);
        } else
            return (-1);
    }
    *ptr = 0;
    return (n);
}

ssize_t
readlinebuf(void **vptrptr) {
    if (read_cnt)
        *vptrptr = read_ptr;
    return (read_cnt);
}
```

## summary

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
