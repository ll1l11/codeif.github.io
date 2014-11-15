---
layout: post
title: ubuntu install squid3, 并搭建安全代理服务器
---


### 安装支持ssl的squid3

    sudo apt-get install devscripts build-essential squid-langpack ssl-cert
    sudo apt-get source squid3
    sudo apt-get build-dep squid3
    cd squid3-3.3.8/

编辑: vim debian/rules

找到 DEB_CONFIGURE_EXTRA_FLAGS

或者搜: enable, 在类似的选项后面添加:

    --enable-ssl


执行: 

    sudo debuild -us -uc
    cd ..
    sudo dpkg -i squid3-common_3.3.8-1ubuntu6.1_all.deb
    sudo dpkg -i squid3_3.3.8-1ubuntu6.1_amd64.deb
    sudo dpkg -i squid3-dbg_3.3.8-1ubuntu6.1_amd64.deb
    

### 配置
配置文件的位置:

    /etc/squid3/squid.conf

修改之前建议先备份一下, 使用vim编辑, 因为行数特别多, 先删掉注释行

    :g/^#/d

删除空白行

    :g/^\s*$/d

配置介绍可以看 <http://wiki.squid-cache.org/SquidFaq/SquidAcl>

加上一行禁用所有缓存:

    cache deny all

生成证书:

    sudo -i  #需要切换到root用户
    openssl req -new -newkey rsa:2048 -nodes -keyout squid.key -out squid.csr
    openssl req -x509 -days 3650 -key squid.key -in squid.csr > squid.crt

最终可以简化成如下的配置:

    acl SSL_ports port 443
    acl Safe_ports port 80      # http
    acl Safe_ports port 21      # ftp
    acl Safe_ports port 443     # https
    acl Safe_ports port 70      # gopher
    acl Safe_ports port 210     # wais
    acl Safe_ports port 1025-65535  # unregistered ports
    acl Safe_ports port 280     # http-mgmt
    acl Safe_ports port 488     # gss-http
    acl Safe_ports port 591     # filemaker
    acl Safe_ports port 777     # multiling http
    acl CONNECT method CONNECT

    http_access deny !Safe_ports
    http_access deny CONNECT !SSL_ports
    http_access allow all

    cache deny all

    https_port 开启的端口 cert=/etc/squid3/squid.crt key=/etc/squid3/squid.key


### 客户端配置

windows mac linux下都有自己的stunnel, 配置基本如下

    pid = /var/run/stunnel4.pid
    options = NO_SSLv2
    [proxy-client]
    client = yes
    accept = 代理的端口
    connect = 远程主机的IP:端口


