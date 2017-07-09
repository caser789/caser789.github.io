---
layout: post
title: Design patterns interpreter
date:   2017-07-09 04:18:01 +0800
categories: pattern design interpreter
tag: pattern
---

* content
{:toc}

## Backus-Naur Form

```
event ::= command token receiver token arguments
command ::= word+
receiver ::= word+
arguments ::= word+
token ::= ->
word ::= a collection of one or more alphanumatic characters
```

## code

```python
from pyparsing import Word
from pyparsing import Group
from pyparsing import OneOrMore
from pyparsing import alphanums
from pyparsing import Suppress
from pyparsing import Optional


class Boiler(object):

    def __init__(self):
        self.temperature = 83

    def __str__(self):
        return "boiler temperature: {}".format(self.temperature)

    def increase_temperature(self, amount):
        print "increasing the boiler's temperature by {} degree".format(amount)
        self.temperature += amount

    def decrease_temperature(self, amount):
        print "decreasing the boiler's temperature by {} degree".format(amount)
        self.temperature -= amount


class Gate(object):

    def __init__(self):
        self.is_open = False

    def __str__(self):
        return "open" if self.is_open else "closed"

    def open(self):
        print "opening the gate"
        self.is_open = True

    def close(self):
        print "closing the gate"
        self.is_open = False


class Garage(object):

    def __init__(self):
        self.is_open = False

    def __str__(self):
        return "open" if self.is_open else "closed"

    def open(self):
        print "opening the garage"
        self.is_open = True

    def close(self):
        print "closing the garage"
        self.is_open = False


class AirCondition(object):

    def __init__(self):
        self.is_on = False

    def __str__(self):
        return "on" if self.is_on else "off"

    def turn_on(self):
        print "turning on the air condition"
        self.is_on = True

    def turn_off(self):
        print "turning off the air condition"
        self.is_on = False


class Heating(object):

    def __init__(self):
        self.is_on = False

    def __str__(self):
        return "on" if self.is_on else "off"

    def turn_on(self):
        print "turning on the heating"
        self.is_on = True

    def turn_off(self):
        print "turning off the heating"
        self.is_on = False


class Fridge(object):

    def __init__(self):
        self.temperature = 2

    def __str__(self):
        return "fridge temperature: {}".format(self.temperature)

    def increase_temperature(self, amount):
        print "increasing the fridge's temperature by {} degree".format(amount)
        self.temperature += amount

    def decrease_temperature(self, amount):
        print "decreasing the fridge's temperature by {} degree".format(amount)
        self.temperature -= amount


def main():
    word = Word(alphanums)
    command = Group(OneOrMore(word))
    token = Suppress("->")
    device = Group(OneOrMore(word))
    argument = Group(OneOrMore(word))
    event = command + token + device + Optional(token + argument)

    #  boiler = Boiler()
    #  print boiler
    #  cmd, device, arg = event.parseString("increase -> boiler temperature -> 3 degrees")
    #  if 'increase' in ' '.join(cmd):
    #      if 'boiler' in ' '.join(device):
    #          boiler.increase_temperature(int(arg[0]))
    #  print boiler

    gate = Gate()
    garage = Garage()
    air = AirCondition()
    heating = Heating()
    boiler = Boiler()
    fridge = Fridge()

    tests = (
        'open -> gate',
        'close -> garage',
        'turn on -> aircondition',
        'turn off -> heating',
        "increase -> boiler temperature -> 5 degrees",
        "decrease -> fridge temperature -> 2 degrees"
    )

    open_actions = {
        'gate': gate.open,
        'garage': garage.open,
        'aircondition': air.turn_on,
        'heating': heating.turn_on,
        'boiler temperature': boiler.increase_temperature,
        'fridge temperature': fridge.increase_temperature
    }

    close_actions = {
        'gate': gate.close,
        'garage': garage.close,
        'aircondition': air.turn_off,
        'heating': heating.turn_off,
        'boiler temperature': boiler.decrease_temperature,
        'fridge temperature': fridge.decrease_temperature
    }

    for t in tests:
        tokens = event.parseString(t)
        length = len(tokens)
        if length == 2:
            cmd, dev = tokens
            cmd_str, dev_str = ' '.join(cmd), ' '.join(dev)
            if 'open' in cmd_str or 'turn on' in cmd_str:
                open_actions[dev_str]()
            elif 'close' in cmd_str or 'turn off' in cmd_str:
                close_actions[dev_str]()
        elif length == 3:
            cmd, dev, args = tokens
            cmd_str, dev_str, args_str = ' '.join(cmd), ' '.join(dev), ' '.join(args)
            num_args = 0
            try:
                num_args = int(args[0])
            except:
                print "invalid arguments {}".format(args)
            if 'increase' in cmd_str and num_args > 0:
                open_actions[dev_str](num_args)
            elif 'decrease' in cmd_str and num_args > 0:
                close_actions[dev_str](num_args)


if __name__ == '__main__':
    main()
```


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
