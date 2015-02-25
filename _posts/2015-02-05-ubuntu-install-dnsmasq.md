---
layout: post
title: ubuntu下安装dnsmasq
---

安装命令:

    sudo apt-get install dnsmasq

帮助:

    man dnsmasq

README位置

    /etc/dnsmasq.d/README

默认配置位置:

    /etc/dnsmasq.conf

重启方式:

    sudo /etc/init.d/dnsmasq restart

或者

    sudo service dnsmasq restart

查看默认启动命令:

    ps aux | grep dnsmasq

看到下面的输出:

    $ ps aux | grep dnsmasq
    nobody    1245  0.0  0.0  35240  1528 ?        S    09:56   0:00 /usr/sbin/dnsmasq --no-resolv --keep-in-foreground --no-hosts --bind-interfaces --pid-file=/run/sendsigs.omit.d/network-manager.dnsmasq.pid --listen-address=127.0.1.1 --conf-file=/var/run/NetworkManager/dnsmasq.conf --cache-size=0 --proxy-dnssec --enable-dbus=org.freedesktop.NetworkManager.dnsmasq --conf-dir=/etc/NetworkManager/dnsmasq.d
    dnsmasq   5977  0.0  0.0  35240   908 ?        S    10:40   0:00 /usr/sbin/dnsmasq -x /var/run/dnsmasq/dnsmasq.pid -u dnsmasq -r /var/run/dnsmasq/resolv.conf -7 /etc/dnsmasq.d,.dpkg-dist,.dpkg-old,.dpkg-new

nobody用户的输出不用管, 其中

    -r /var/run/dnsmasq/resolv.conf

是dnsmasq用到的dns, 我们运营商提供的dns,例如

    nameserver 180.76.76.76
    nameserver 114.114.114.114

