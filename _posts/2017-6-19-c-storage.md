---
layout: post
title: C Storage Classes
date:   2017-06-19 13:18:01 +0100
categories: c storage class
tag: c
---

* content
{:toc}


## auto
```
{
    int mount;
    auto int month;
}
```
auto can only used with local variables;
## register
```
{
    register int miles;
}
```
cannot use '&'
size == one word
## static
keep a local variable during life-time of program
* maintain values between function calls
* make global variable's scope to one file
```c
#include <stdio.h>

void func(void);

static int count = 5;

main() {
    while(count--) {
        func();
    }
    return 0;
}

void func(void) {
    static int i = 5;
    i++;
    printf("i is %d and count is %d\n", i, count);
}
```
## extern

global variable among files
```c
#include <stdio.h>

int count;
extern void write_extern();
main() {
    count = 5;
    write_extern();
}
```
```c
#include <stdio.h>

extern int count;

void write_extern(void) {
    printf("count is %d\n", count);
}
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
