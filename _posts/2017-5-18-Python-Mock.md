---
layout: post
title: Mock in Python
date:   2017-05-22 06:05:01 +0100
categories: python test mock
tag: mock
---

* content
{:toc}

## install
```
pip install mock
```


## import
```
import mock
from unittest import mock
```

## mock a method

```
class EventTest(unittest.TestCase):
    def test_a_listener_is_notified_when_an_event_is_raised(self):
        listener = mock.Mock()
        event = Event()
        event.connect(listener)
        event.fire()

        self.assertTrue(listener.called)
        listener.assert_called_with(5, shape="square")
```

## most used methods
```
assert_called_with
assert_called_once_with
assert_any_call
assert_has_calls
```

## MagicMock vs Mock

1. A MagicMock object is like a Mock object but it has special support for Python's magic methods which are present on all classes, such as __str__, hasattr
2. Use MagicMock to mock a class
```
>>> from unittest import mock
>>> mock_1 = mock.Mock()
>>> mock_2 = mock.MagicMock()
>>> len(mock_1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: object of type 'Mock' has no len()
>>> len(mock_2)
0
>>>
```

## mock return values

* way one

```
>>> matches = mock.Mock(return_value=True)
>>> matches()
True
>>> matches(4)
True
>>> matches(4, "abcd")
True
```

* way two
```
>>> from unittest import mock
>>> rule = mock.MagicMock()
>>> rule.matches.return_value = True
>>> rule.matches()
True
>>>
```

## mock side effect


* We may want to call another method, like in the case of the Stock class, which needs to fire the event when the update method is called.
```
def test_action_is_executed_when_rule_matches(self):
    goog = mock.MagicMock(spec=Stock)
    goog.updated = Event()
    goog.update.side_effect = lambda date, value: goog.updated.fire(self)
    exchange = {"GOOG": goog}
    rule = mock.MagicMock(spec=PriceRule)
    rule.matches.return_value = True
    rule.depends_on.return_value = {"GOOG"}
    action = mock.MagicMock()
    alert = Alert("sample alert", rule, action)
    alert.connect(exchange)
    exchange["GOOG"].update(datetime(2014, 2, 10), 11)
    action.execute.assert_called_with("sample alert")
```
* To raise an exception: this is particularly useful when testing error situations. Some errors such as a network timeout might be very difficult to simulate, and it is better to test using a mock that simply raises the appropriate exception.
```
>>> m = mock.Mock()
>>> m.side_effect = Exception()
>>> m()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python34\lib\unittest\mock.py", line 885, in __call__
    return _mock_self._mock_call(*args, **kwargs)
  File "C:\Python34\lib\unittest\mock.py", line 941, in _mock_call
```
* To return multiple values: these may be different values each time the mock is called, or specific values, depending on the parameters passed.

```
>>> m = mock.Mock()
>>> m.side_effect = [1, 2, 3]
>>> m()
1
>>> m()
2
>>> m()
3
>>> m()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python34\lib\unittest\mock.py", line 885, in __call__
    return _mock_self._mock_call(*args, **kwargs)
  File "C:\Python34\lib\unittest\mock.py", line 944, in _mock_call
    result = next(effect)
StopIteration
```

## mocks v.s. stubs v.s. fakes v.s. spies

* Mocks are a speci c kind of test double that record information about calls that have been made to it, so that we can assert on them later.
* Stubs are just an empty do-nothing kind of object or method. They are used when we don't care about some functionality in the test.
* Fakes are a replacement of one object or system with a simpler one that facilitates easier testing. In-memory database
* Spies are objects that are like middlemen.

## patching
* way one

```
def test_executing_action_prints_message(self):
    patcher = mock.patch('builtins.print')
    mock_print = patcher.start()
    try:
        action = PrintAction()
        action.execute("GOOG > $10")
        mock_print.assert_called_with("GOOG > $10")
finally:
   patcher.stop()
```
* way two
```
def test_executing_action_prints_message(self):
    with mock.patch('builtins.print') as mock_print:
        action = PrintAction()
        action.execute("GOOG > $10")
        mock_print.assert_called_with("GOOG > $10")
```
* way three
```
@mock.patch("builtins.print")
def test_executing_action_prints_message(self, mock_print):
    action = PrintAction()
    action.execute("GOOG > $10")
    mock_print.assert_called_with("GOOG > $10")
```

## use fixture instead of decorator
```
@mock.patch.object(smtplib, "SMTP")
class EmailActionTest(unittest.TestCase):
    def setUp(self):
        self.action = EmailAction(to="abs@163.com")

    def test_connection_closed_after_sending_mail(self, mock_smtp_class):
        mock_smtp = mock_smtp_class.return_value
        self.action.execute("MSFT has crossed $10 price level")
        mock_smtp.send_message.assert_called_with(mock.ANY)
        self.assertTrue(mock_smtp.quit.called)
        mock_smtp.assert_has_calls([mock.call.send_message(mock.ANY), mock.call.quit()])
```

```
class EmailActionTest(unittest.TestCase):
    def setUp(self):
        patcher = mock.patch("smtplib.SMTP")
        self.addCleanup(patcher.stop)
        self.mock_smtp_class = patcher.start()
        self.mock_smtp = self.mock_smtp_class.return_value
        self.action = EmailAction(to="abs@163.com")

    def test_connection_closed_after_sending_mail(self):
        self.action.execute("MSFT has crossed $10 price level")
        self.mock_smtp.send_message.assert_called_with(mock.ANY)
        self.assertTrue(self.mock_smtp.quit.called)
        self.mock_smtp.assert_has_calls([mock.call.send_message(mock.ANY), mock.call.quit()])
```

## mock attribute

```python
@ddt.data("id", "artStyle", "bodyshape", "clothesChanged", "clothesID", "gender", "name", "changedOrder", "avatarUnits")
@mock.patch('dock_zmoji.view.zmoji.Zmoji')
@mock.patch('dock.web.helpers.g')
def test_avatars_put_parameter_error_by_post_pop_units(self, key, mock_g, mock_Zmoji):
    req_data = copy.deepcopy(PUT_REQ_DATA)
    url = '/zmoji/avatars/put'
    for avatar in req_data['avatars']:
        avatar['selectionData'].pop(key, None)
    mock_g_config = {
        'account': mock.MagicMock(**PUT_REQ_DATA),
        'jsondata': copy.deepcopy(req_data)
    }
    with mock.patch('dock_zmoji.view.zmoji.g', **mock_g_config) as mock_login_g:
        mock_login_g.account = mock.MagicMock()
        mock_zmoji = mock_Zmoji.return_value
        resp = self.app.post(url, data=self.encrypt_request(req_data))
        mock_Zmoji.assert_called_once()
        mock_zmoji.put_avatars.assert_not_called()
        resp_data = json.loads(resp.data)
        self.assertEqual(resp_data['meta']['code'], 400)

```
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
