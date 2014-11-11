---
layout: post
title: 给现有的nginx添加concat模块
---

安装步骤参考: <http://etosun.com/nginx-to-tengine.html>

如果ubuntu上有的模块不存在, 参考 <http://kx.cloudingenium.com/linux/ubuntu/build-version-nginx/>

基本步骤:

到 [Tengine](http://tengine.taobao.org/) 的官方下载你需要的tengine的版本

然后解压, 进入接下目录, 可以看到 configure

先找到原来nginx的编译参数:

    nginx -V


例如我的机器上得到如下的返回

    configure arguments: --with-cc-opt='-g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-ipv6 --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_spdy_module --with-http_sub_module --with-http_xslt_module --with-mail --with-mail_ssl_module


我们只需要--prefix后面的编译项, 然后在--with-debug后面加上 --with-http_concat_module, 后面如果编译不通过看下是否有 --add-module的编译项,如果存在可以删掉之后的尝试, 最后编译命令类似如下:

    ./configure --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-http_concat_module --with-pcre-jit --with-ipv6 --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_spdy_module --with-http_sub_module --with-http_xslt_module --with-mail --with-mail_ssl_module

如果编译缺少依赖, 安装相应依赖,例如:

PCRE library

    apt-get install libpcre3 libpcre3-dev
    
C++ Compiler

    apt-get install build-essential

OpenSSL

    sudo apt-get install openssl libssl-dev libperl-dev

Xml xslt

    apt-get install libxslt-dev

GeoIP Library

    apt-get install libgeoip-dev

执行完上面的命令后, 继续执行make(不用执行make install), 执行完后用我们编译出来的nginx代替原来的就可以


对原来的进行备份:

    cp /usr/bin/nginx /usr/bin/nginx.bak

停掉nginx服务

    service nginx stop

复制nginx到原来nginx的位置

    cd objs/
    cp nginx /usr/bin/nginx

然后启动nginx就可以了.
