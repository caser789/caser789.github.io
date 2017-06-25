---
layout: post
title: Design patterns proxy
date:   2017-06-25 09:3:01 +0800
categories: pattern design state
tag: pattern
---

* content
{:toc}


## 目标

1. 允许对象根据内在状态改变行为，看上去对象改变了自己的类
2. 面向对象的状态机

## 针对的问题

1. 大量的 cases
2. 根据状态动态改变行为

## 2 个地方改变状态

1. 在一个状态中改变状态，好处是增加新状态容易，缺点是一个状态依赖其他状态

## 与其他模式关系


1. state 对象通常是 singleton
2. strategy 模式对算法的选择跟状态无关，跟策略有关。而 state 模式更动态一点

## 实现

```
"""
Allow an object to alter its behavior when its internal state changes.
The object will appear to change its class
"""
import abc


class Context:
    """
    Define the interface of interest to clients.
    Maintain an instance of a ConcreteState subclass that defines the current state
    """
    def __init__(self, state):
        self._state = state

    def request(self):
        self._state.handle()


class State(metaclass=abc.ABCMeta):
    """
    Define an interface for encapsulating the behavior associated with a
    paricular state of the Context
    """
    @abc.abstractmethod
    def handle(self):
        pass


class ConcreteStateA(State):
    """
    Implement a behavior associated with a state of the Context
    """
    def handle(self):
        pass


class ConcreteStateB(State):
    """
    Implement a behavior associated with a state of the Context
    """
    def handle(self):
        pass


def main():
    concrete_state_a = ConcreteStateA()
    context = Context(concrete_state_a)
    context.request()


if __name__ == '__main__':
    main()
```

## example

```
import abc


class Worker(object):

    def __init__(self, state):
        self._state = state
        self.hour = 0

    def work(self):
        self._state.handle(self.hour)


class State(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def handle(self, hour):
        pass


class MorningState(State):
    def handle(self, hour):
        if hour < 11:
            print 'Present hour: {0}, in the morning, I\'m on fire'.format(hour)
        else:
            NoonState().handle(hour)


class NoonState(State):
    def handle(self, hour):
        if hour < 13:
            print 'Present hour: {0}, in the noon, I\'m hungry and sleepy'.format(hour)
        else:
            AfterNoonState().handle(hour)


class AfterNoonState(State):
    def handle(self, hour):
        if hour < 17:
            print 'Present hour: {0}, in the afternoon, refreshed now'.format(hour)
        else:
            EveningState().handle(hour)


class EveningState(State):
    def handle(self, hour):
        if hour < 21:
            print 'Present hour: {0}, in the evening, tired'.format(hour)
        else:
            SleepingState().handle(hour)


class SleepingState(State):
    def handle(self, hour):
        print 'Present hour: {0}, in the evening, go to bed'.format(hour)


class ResetState(State):
    def handle(self, hour):
        print 'Present hour: {0}, in the evening, reset'.format(hour)


def main():
    morning_state = MorningState()
    xuejiao = Worker(morning_state)
    xuejiao.work()
    xuejiao.hour = 12
    xuejiao.work()
    xuejiao.hour = 14
    xuejiao.work()
    xuejiao.hour = 18
    xuejiao.work()
    xuejiao.hour = 22
    xuejiao.work()

if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
