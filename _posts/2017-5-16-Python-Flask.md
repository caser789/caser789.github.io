---
layout: post
title: Flask Usage Cases
date:   2017-05-16 10:27:01 +0800
categories: python flask unittest
tag: flask
---

* content
{:toc}

Test Client
====================

## Get with querystring in json

```python
import unittest
import hmac
import hashlib
from dock_zmoji.server import app
from dock.common import log
from dock.common import config
from dock.common import json
logger = log.get_logger('tests.zmoji.view.test_zmoji')


class TestCase(unittest.TestCase):

    def setUp(self):
        app.flaskapp.config['TESTING'] = True
        self.app = app.flaskapp.test_client()

    @staticmethod
    def encrypt_request(req_data):
        app_id = config.apps.keys()[-1]
        sig_kv = config.apps[app_id]['sig_keys'].keys()[-1]
        sig_key = config.apps[app_id]['sig_keys'][sig_kv]
        content = json.dumps(req_data)
        sig = hmac.new(sig_key, content, hashlib.sha256).hexdigest()
        return {'content': content, 'signature': sig, 'sig_kv': sig_kv}

    def test_settings_put_success_by_get(self):
        url = '/zmoji/settings/put'
        req_data = {
            "mid": "demo_mid",
            "app_id": 444,
            "settings": {
                "AllInOne": "NO",
                "KeyboardSettings": {
                    "predictive": "YES",
                    "languages": "en-us-qwerty",
                    "emojiskin": 3
                }
            }
        }
        resp = self.app.get(url, query_string=self.encrypt_request(req_data))
        print resp
        print dir(resp)

    def test_settings_put_success_by_post(self):
        url = '/zmoji/settings/put'
        req_data = {
            "mid": "demo_mid",
            "app_id": 444,
            "settings": {
                "AllInOne": "NO",
                "KeyboardSettings": {
                    "predictive": "YES",
                    "languages": "en-us-qwerty",
                    "emojiskin": 3
                }
            }
        }
        resp = self.app.post(url, data=self.encrypt_request(req_data))
        print resp
        print dir(resp)
```

## refs

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
