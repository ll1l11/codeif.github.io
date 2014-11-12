---
layout: post
title: httplib2 proxy设置, https错误解决
---


httplib2例子: <https://code.google.com/p/httplib2/wiki/Examples>


基本使用:

    http = httplib2.Http()
    resp, content = http.request('https://github.com/', 'GET')
    print resp

使用代理:

    http = httplib2.Http(proxy_info=httplib2.ProxyInfo(socks.PROXY_TYPE_HTTP, 'localhost', 8888))
    resp, content = http.request('https://github.com', 'GET')
    print resp


如果报错, 下载[socks.py](http://sourceforge.net/projects/socksipy/?source=dlp), 放到当前目录或者PYTHONPATH下


使用代理抓包时可能报下面的错误:

    Traceback (most recent call last):
      File "test.py", line 20, in <module>
        proxy_test()
      File "test.py", line 15, in proxy_test
        resp, content = http.request('https://www.googleapis.com/', 'GET')
      File "/Users/xyz/.virtualenvs/dev/lib/python2.7/site-packages/httplib2/__init__.py", line 1593, in request
        (response, content) = self._request(conn, authority, uri, request_uri, method, body, headers, redirections, cachekey)
      File "/Users/xyz/.virtualenvs/dev/lib/python2.7/site-packages/httplib2/__init__.py", line 1335, in _request
        (response, content) = self._conn_request(conn, request_uri, method, body, headers)
      File "/Users/xyz/.virtualenvs/dev/lib/python2.7/site-packages/httplib2/__init__.py", line 1257, in _conn_request
        conn.connect()
      File "/Users/xyz/.virtualenvs/dev/lib/python2.7/site-packages/httplib2/__init__.py", line 1044, in connect
        raise SSLHandshakeError(e)
    httplib2.SSLHandshakeError: [Errno 1] _ssl.c:507: error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed

解决方法, 添加disable_ssl_certificate_validation=True, 代码如下:

    http = httplib2.Http(proxy_info=httplib2.ProxyInfo(socks.PROXY_TYPE_HTTP, 'localhost', 8888), disable_ssl_certificate_validation=True)

