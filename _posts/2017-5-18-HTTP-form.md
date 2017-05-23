---
layout: post
title: HTTP form
date:   2017-05-23 07:31:01 +0100
categories: http form
tag: http
---

* content
{:toc}


## Requirements

* flask, httpie, ngrok
* start server `python app.py`
* start ngrok `ngrok http 5000`
* open browser `localhost:4040`

## Content-Type

* HTTP 的 content-type 头用来描述 HTTP body 的 MIME 类型，常见的有 text/html，image/jpeg 等，应用程序根据这个头来确定如何处理 HTTP 报文的内容
* 当客户端向服务器发送 post 请求时，常见的 content-type 有： application/x-www-form-urlencoded, multipart/form-data, text/plain 等


### application/x-www-form-urlencoded

* 在浏览器的表单中，如果 enctype 属性没有设置，默认就是此类型
* HTTP 报文体中的数据会被转为 query string 类似的数据格式发送


### multipart/form-data

* urlencode 编码会将一些字符转为三个字符表示，当发送的数据量较大时，效率比较低
* multipart/form-data 这种方式会将发送的数据分成几段来发送
* 各段之间用 boundary 隔开
* bounday 是一个 string，为避免混淆，这个 string 不应当出现在该报文中
* 每一段包含的header:

    * Content-Disposition: 该段内容的一些额外信息，告诉应用程序如何处理该内容
    * Content-Disposition: form-data; name="file1"; filename="filename.txt"
    * Content-Type: 该段内容的content类型

### application/json

## demo cmds

application/json

http -j POST 127.0.0.1:5000/demo1 a=b c=d -v

http -f POST 127.0.0.1:5000/demo1 a=b c="测 试" -v

http -f POST 127.0.0.1:5000/demo1 a=b c="测 试" Content-Type:plain/text -v

http -f POST 127.0.0.1:5000/demo1 a@file1.txt c@file2.txt c=d -v

http -f POST 127.0.0.1:5000/demo2 a=b c="测 试" -v

## demo server
```python
from flask import Flask, request, render_template
app = Flask(__name__)


@app.route("/demo1", methods=['GET', 'POST'])
def demo1():
    if request.method == 'GET':
        return render_template('demo1.html')
    if request.method == 'POST':
        args = request.args.to_dict()
        form = request.form.to_dict()
        values = request.values.to_dict()
        data = request.data
        data = ('args is %s<br> forms is %s <br> values is %s<br> data is %s<br>' %
                (args, form, values, data))
        return data


@app.route("/demo2", methods=['GET', 'POST'])
def demo2():
    if request.method == 'GET':
        return render_template('demo2.html')
    if request.method == 'POST':
        args = request.args.to_dict()
        form = request.form.to_dict()
        values = request.values.to_dict()
        data = request.data
        files = [request.files['file1'].filename, request.files['file2'].filename]

        data = ('args is %s<br> forms is %s <br> values is %s<br> data is %s<br> files is %s <br>' %
                (args, form, values, data, files))
        return data

if __name__ == '__main__':
    app.run(debug=True)
```

# refs

[URL encoding](http://blog.lunatech.com/2009/02/03/what-every-web-developer-must-know-about-url-encoding)


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
