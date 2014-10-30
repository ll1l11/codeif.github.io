---
layout: post
title: max os x下安装libxml2
---

在 mac os x下安装的virtualenv环境下安装 pyquery, 

    pip install pyquery

报错:


    cc -fno-strict-aliasing -fno-common -dynamic -arch x86_64 -arch i386 -g -Os -pipe -fno-common -fno-strict-aliasing -fwrapv -DENABLE_DTRACE -    DMACOSX -DNDEBUG -Wall -Wstrict-prototypes -Wshorten-64-to-32 -DNDEBUG -g -fwrapv -Os -Wall -Wstrict-prototypes -DENABLE_DTRACE -arch x86_64    -arch i386 -pipe -I/usr/include/libxml2 -I/Users/xyz/.virtualenvs/dev/build/lxml/src/lxml/includes -I/System/Library/Frameworks/Python.    framework/Versions/2.7/include/python2.7 -c src/lxml/lxml.etree.c -o build/temp.macosx-10.10-intel-2.7/src/lxml/lxml.etree.o -w -   flat_namespace
    
    In file included from src/lxml/lxml.etree.c:232:
    
    /Users/xyz/.virtualenvs/dev/build/lxml/src/lxml/includes/etree_defs.h:14:10: fatal error: 'libxml/xmlversion.h' file not found
    
    #include "libxml/xmlversion.h"
    
             ^
    
    1 error generated.
    
    error: command 'cc' failed with exit status 1

使用pip安装libxml2报上面错误, 使用 brew安装

    brew install libxml2

安装结果如下:

    ==> Downloading http://xmlsoft.org/sources/libxml2-2.9.1.tar.gz
    ######################################################################## 100.0%
    ==> ./configure --prefix=/usr/local/Cellar/libxml2/2.9.1 --without-python --without-lzma
    ==> make
    ==> make install
    ==> Caveats
    This formula is keg-only, which means it was not symlinked into /usr/local.
    
    Mac OS X already provides this software and installing another version in
    parallel can cause all kinds of trouble.
    
    Generally there are no consequences of this for you. If you build your
    own software and it requires this formula, you'll need to add to your
    build variables:
    
        LDFLAGS:  -L/usr/local/opt/libxml2/lib
        CPPFLAGS: -I/usr/local/opt/libxml2/include
    
    ==> Summary
    🍺  /usr/local/Cellar/libxml2/2.9.1: 274 files, 11M, built in 2.9 minutes 

然后安装lxml还是报错

    pip install lxml

网上搜到一篇文章<http://www.eightytwo.com.au/notes/osx/installing-lxml-on-mavericks/>
配置 C\_INCLUDE\_PATH 环境变量(将下面的环境变量添加到.bash_profile下)

    export C_INCLUDE_PATH=/usr/local/Cellar/libxml2/2.9.1/include/libxml2:$C_INCLUDE_PATH

然后执行:
    
    source ~/.bash_profile

这样就可以使用pip的安装 lxml 和 pyquery 了

