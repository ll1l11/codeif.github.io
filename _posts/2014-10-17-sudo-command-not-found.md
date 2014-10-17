---
layout: post
titile: ubuntu给命令添加sudo权限
---


手动安装nginx到/opt/nginx目录下, 在.bashrc配置了环境变量, 用普通用户执行

    sudo nginx -t

时报错:

    sudo: nginx: command not found

解决方法:

1. 编辑 sudo vim /etc/sudoers

在下面一行的最后加上:/opt/nginx/sbin
    
    Defaults   secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

如下

    Defaults    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/nginx/sbin"

然后保存就可以了

可以把我们原先的.bashrc中的环境变量删除了

