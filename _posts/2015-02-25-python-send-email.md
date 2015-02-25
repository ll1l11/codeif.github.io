---
layout: post
title: python发送email
---

参考: <http://blog.csdn.net/dbanote/article/details/8924686>

因为163的邮箱开通smtp还需要绑定手机, 所以这里使用qq的邮箱, 最简单的代码如下
进入QQ邮箱, 开启stmp服务, 可能需要设置独立密码, 下面会用到

邮箱服务地址和端口看: <http://kf.qq.com/faq/120322fu63YV130422nqIrqu.html>


    # -*- coding:  utf-8 -*-
    import smtplib

    server = smtplib.SMTP('smtp.qq.com', 25)
    server.login('qq或者qq@qq.com', '邮箱独立密码')

    msg = '\nHello!'
    server.sendmail('from@qq.com', 'to@qq.com', msg)
