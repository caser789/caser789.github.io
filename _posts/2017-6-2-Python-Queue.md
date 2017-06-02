---
layout: post
title: Access the same resource with multi-threads in python
date:   2017-06-02 03:18:01 +0100
categories: python queue thread lock gevent
tag: lock
---

* content
{:toc}


## Use a lock to access the same resource in multi-threads

```python
from gevent import sleep
from gevent.pool import Pool
from gevent.lock import Semaphore

sem = Semaphore(2)


def worker1(n):
    sem.acquire()
    print('Worker %i acquired semaphore' % n)
    sleep(0)
    sem.release()
    print('Worker %i released semaphore' % n)


def worker2(n):
    with sem:
        print('Worker %i acquired semaphore' % n)
        sleep(0)
    print('Worker %i released semaphore' % n)


pool = Pool()
pool.map(worker1, xrange(0, 2))
pool.map(worker2, xrange(3, 6))
```

## Use a queue to access the same resource in multi-threads
```python
import os
import Queue
import threading


class PrintThread(threading.Thread):

    def __init__(self, queue):
        threading.Thread.__init__(self)
        self.queue = queue

    def run(self):
        while True:
            result = self.queue.get()
            self.printfiles(result)
            self.queue.task_done()

    def printfiles(self, p):
        for path, dirs, files in os.walk(p):
            for f in files:
                print '*' * 10
                print f
                print '*' * 10


class ProcessThread(threading.Thread):

    def __init__(self, in_queue, out_queue):
        threading.Thread.__init__(self)
        self.in_queue = in_queue
        self.out_queue = out_queue

    def run(self):
        while True:
            path = self.in_queue.get()
            result = self.process(path)
            self.out_queue.put(result)
            self.in_queue.task_done()

    def process(self, p):
        return p


def main():
    path_queue = Queue.Queue()
    result_queue = Queue.Queue()
    paths = ['/tmp']

    for i in range(5):
        t = ProcessThread(path_queue, result_queue)
        t.setDaemon(True)
        t.start()

    t = PrintThread(result_queue)
    t.setDaemon(True)
    t.start()

    for path in paths:
        path_queue.put(path)

    path_queue.join()
    result_queue.join()


if __name__ == '__main__':
    main()
```


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
