---
layout: post
title: System Programming Concepts
date:   2017-06-01 11:18:01 +0100
categories: tlpi system linux unix
tag: tlpi
---

* content
{:toc}

## System Call

* A system call changes the processor state from user mode to kernel model, so that the CPU can access protected kernel
    memory.
* The set of system calls is fixed. Each system call is identified by a unique number. (CPU knows the system call as a
    number whereas app knows the system call as a name)
* Each system call may have a set of argument that specify information to be transferred from user space (i.e.; the process's virtual address space) to kernel space
    and vice versa.










[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
