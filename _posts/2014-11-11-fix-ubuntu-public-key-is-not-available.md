---
layout: post
title: 修复ubuntu下public key is not available错误
---

在ubuntu中使用apt-get source获取nginx的源码报下面的错误:

    $ sudo apt-get source nginx
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    NOTICE: 'nginx' packaging is maintained in the 'Git' version control system at:
    git://anonscm.debian.org/collab-maint/nginx.git
    WARNING: The following packages cannot be authenticated!
      nginx
    E: Some packages could not be authenticated
    ubuntu@ip-10-203-128-183:~/weiwei/nginx_source$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys KEY
    Executing: gpg --ignore-time-conflict --no-options --no-default-keyring --homedir /tmp/tmp.9FBjZs5IqX --no-auto-check-trustdb --trust-model always --keyring /etc/apt/trusted.gpg --primary-keyring /etc/apt/trusted.gpg --keyserver keyserver.ubuntu.com --recv-keys KEY


执行apt-get update最后会报下面的错误:

    Reading package lists... Done
    W: GPG error: http://security.ubuntu.com trusty-security Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 40976EAF437D05B5 NO_PUBKEY 3B4FE6ACC0B21F32
    W: GPG error: http://us-west-1.ec2.archive.ubuntu.com trusty Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 40976EAF437D05B5 NO_PUBKEY 3B4FE6ACC0B21F32
    W: GPG error: http://us-west-1.ec2.archive.ubuntu.com trusty-updates Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 40976EAF437D05B5 NO_PUBKEY 3B4FE6ACC0B21F32

解决方法:

    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32


然后再执行apt-get update 也不会出现上面的问题了


