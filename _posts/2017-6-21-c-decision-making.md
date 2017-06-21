---
layout: post
title: C Decision making
date:   2017-06-21 12:18:01 +0100
categories: c if switch else
tag: decision
---

* content
{:toc}

## if
```
if (boolean_expression) {
    /* statement(s) */
}
```

```c
#include <stdio.h>
int main() {
    /* local variable definition */
    int a = 10;

    /* check the boolean condition using if statement */

    if (a < 20) {
        printf("a is less than 20\n");
    }

    printf("value of a is: %d\n", a);
    return 0;
}
```

## else
```
if(boolean_expression) {
    /* statement(s) */
} 
else {
    /* statement(s) */
}
```

```c
#include <stdio.h>

int main() {
    int a = 100;
    if (a < 20) {
        printf("a is less than 20\n");
    }
    else {
        printf("a is not less than 20\n");
    }
    printf("value of a is : %d\n", a);
    return 0;
}
```
##  if-else-if
```
if(boolean_expression 1) {
    /* statement(s) */
}
else if(boolean_expression 2) {
    /* statement(s) */
}
else if(boolean_expression 3) {
    /* statement(s) */
}
else {
    /* statement(s) */
}
```

```c
#include <stdio.h>

int main() {
    int a = 100;
    if(a == 10) {
        printf("Value of a is 10\n");
    }
    else if(a == 20) {
        printf("Value of a is 20\n");
    }
    else if(a == 30) {
        printf("Value of a is 30\n");
    }
    else if(a == 40) {
        printf("Value of a is 40\n");
    }
    else if(a == 50) {
        printf("Value of a is 50\n");
    }
    else if(a == 60) {
        printf("Value of a is 60\n");
    }
    else if(a == 70) {
        printf("Value of a is 70\n");
    }
    else if(a == 80) {
        printf("Value of a is 80\n");
    }
    else if(a == 90) {
        printf("Value of a is 90\n");
    }
    else {
        printf("None of the values is matching\n");
    }
    printf("Exact value of a is: %d\n", a);
    return 0;
}
```
##  switch
```c
switch(expression) {
    case constant-expression:
        statement(s);
        break;
    case constant-expression:
        statement(s);
        break;
    default:
        statement(s);
}
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
