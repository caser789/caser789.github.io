---
layout: post
title: C function
date:   2017-06-22 04:18:01 +0100
categories: c function
tag: c
---

* content
{:toc}

## defining a function

```
return_type function_name(parameter list) {
    body of the function
}
```
```c
#include <stdio.h>

int max(int num1, int num2);

int main() {
    int a = 100;
    int b = 200;
    int ret;

    ret = max(a, b);
    printf("Max value is: %d\n", ret);
    return 0;
}

int max(int num1, int num2) {
    int result;
    if (num1 > num2)
        result = num1;
    else
        result = num2;
    return result;
}
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
