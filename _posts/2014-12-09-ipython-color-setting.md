---
layout: post
title: 修改ipython提示符的颜色
---

发现有的电脑ipython的提示符是绿色, 有的是蓝色

配置方式:

进入目录:

    ~/.ipython/profile_default

找到文件 ipython_config.py

如果没有, 执行:

    ipython profile create

然后找到:


    # Set the color scheme (NoColor, Linux, or LightBG).
    # c.TerminalInteractiveShell.colors = 'LightBG'
    c.TerminalInteractiveShell.colors = 'Linux'


然后改成不同的选项试试就可以了
