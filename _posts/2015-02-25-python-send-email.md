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

msg需要\n开头, 否则看不到正文

如果使用ssl的加密方式, 参考: <http://stackoverflow.com/questions/64505/sending-mail-from-python-using-smtp>

代码如下:


    server = smtplib.SMTP('smtp.qq.com', 587)
    server.ehlo()
    server.starttls()
    server.ehlo()
    server.login('username', 'passwd')



一个带主题和内容的例子


    # -*- coding: utf-8 -*-
    import smtplib
    from email.MIMEMultipart import MIMEMultipart
    from email.MIMEText import MIMEText

    fromaddr = 'from@qq.com'
    toaddr = 'to@qq.com'
    ccaddr = 'cc@qq.com'

    msg = MIMEMultipart()
    msg['From'] = fromaddr
    msg['To'] = toaddr
    msg['Cc'] = ccaddr
    msg['Subject'] = 'this is subject'

    body = 'this is body'
    msg.attach(MIMEText(body, 'plain'))

    text = msg.as_string()
    print text


    server = smtplib.SMTP('smtp.qq.com', 25)
    server.login('username', 'passwd')
    server.sendmail(fromaddr, toaddr, text)

如果发送html邮件, 如下写:

    body = '<a href="http://www.codeif.com/">codeif</a>'
    msg.attach(MIMEText(body, 'html'))

邮件中带有附件:

    # 附件
    att = MIMEText(open('/Users/xyz/1.txt', 'rb').read(), 'base64', 'utf-8')
    att['Content-Type'] = 'application/octet-stream'
    att['Content-Disposition'] = 'attachment; filename="123.txt"'
    msg.attach(att)

