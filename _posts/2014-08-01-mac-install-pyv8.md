---
layout: post
title: mac下安装pyv8
tags: [python, js]
---

安装说明:
<https://code.google.com/p/pyv8/wiki/HowToBuild#Boost>

系统安装v8:

    brew install v8

如果不安装v8会报下面的错误:
    
    src/Exception.h:6:16: fatal error: v8.h: 没有那个文件或目录

安装说明有这么一句:

    PyV8 use boost::python for interoperability, so, download the latest boost version and build the library.

所以安装boost, 安装方法参见:<http://stackoverflow.com/questions/104322/how-do-you-install-boost-on-macos>, 安装命令:

    brew install boost


如果不安装boost会报下面的错误:

    src/Exception.h:16:10: fatal error: 'boost/python.hpp' file not found