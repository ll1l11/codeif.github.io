---
layout: post
title: max下安装pylucene
---

官方文档:<http://lucene.apache.org/pylucene/install.html>

需要安装jdk1, 下载地址: <http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html>

安装常见问题: <http://docs.oracle.com/javase/7/docs/webnotes/install/mac/mac-jdk.html>

mac下载软件断点续传使用QQ浏览器

安装jdk1.7后, 电脑中存在两个jdk版本:

    w3@mac:/Library/Java/JavaVirtualMachines > ls
    1.6.0_43-b01-447.jdk jdk1.7.0_60.jdk

默认的jdk版本已经是1.7了

    w3@mac:~ > java -version
    java version "1.7.0_60"
    Java(TM) SE Runtime Environment (build 1.7.0_60-b19)
    Java HotSpot(TM) 64-Bit Server VM (build 24.60-b09, mixed mode) 

如何切换到版本vim ~/.bashrc?

    export JAVA_HOME=`/usr/libexec/java_home -v 1.6`

下载pylucene <http://www.apache.org/dyn/closer.cgi/lucene/pylucene/>

进入jcc目录, 安装jcc <http://lucene.apache.org/pylucene/jcc/install.html>



未完,待续




