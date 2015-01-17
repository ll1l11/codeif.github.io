---
layout: post
title: ubuntu安装supervisor
---

官网地址:<http://supervisord.org/>

使用pip安装

    pip install supervisor --pre


我们使用开机自启动脚本启动,所以配置文件不放在默认位置:

    echo_supervisord_conf > /etc/supervisor/supervisord.conf


然后使用vim编辑/etc/supervisor/supervisord.conf, 主要是改下输出文件的位置, 例如:

    [unix_http_server]
    file=/var/run/supervisor/supervisor.sock   ; (the path to the socket file)

    [supervisord]
    logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
    pidfile=/var/run/supervisor/supervisord.pid ; (supervisord pidfile;default supervisord.pid)

    [supervisorctl]
    serverurl=unix:///var/run/supervisor/supervisor.sock ; use a unix:// URL  for a unix socket

    [include]
    files = /etc/supervisor/conf.d/*.ini

我们把要启动的程序添加到 /etc/supervisor/conf.d/ 下就可以了


###配置开机自启动###

可以在这下载开机脚本 <https://github.com/Supervisor/initscripts> 将ubuntu文件保存为 /etc/init.d/supervisord

不过我们要改下脚本的位置,比如我的改成了如下(改为对应自己的就可以)

    DAEMON=/usr/local/bin/supervisord
    SUPERVISORCTL=/usr/local/bin/supervisorctl


执行下面的命令:

    sudo chmod +x /etc/init.d/supervisord
    sudo update-rc.d supervisord defaults
    sudo /etc/init.d/supervisord start
    

