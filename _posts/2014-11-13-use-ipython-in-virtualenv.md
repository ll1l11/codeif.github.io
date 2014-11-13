---
layout: post
title: 在virtualenv中使用ipython
---


在virtualenv的虚拟环境中, 默认是不使用virtuanlenv的PYTHONPATH(sys.path)的, 如下:

    p3@p3-ubuntu:~$ workon dev
    (dev)p3@p3-ubuntu:~$ ipython
    WARNING: Attempting to work in a virtualenv. If you encounter problems, please install IPython inside the virtualenv.
    Python 2.7.6 (default, Mar 22 2014, 22:59:56)
    Type "copyright", "credits" or "license" for more information.
    
    IPython 2.0.0 -- An enhanced Interactive Python.
    ?         -> Introduction and overview of IPython's features.
    %quickref -> Quick reference.
    help      -> Python's own help system.
    object?   -> Details about 'object', use 'object??' for extra details.
    
    In [1]: import sys
    
    In [2]: sys.path
    Out[2]:
    ['/home/p3/.virtualenvs/dev/lib/python2.7/site-packages',
     '',
     '/usr/local/bin',
     '/usr/local/lib/python2.7/dist-packages/gitosis-0.2-py2.7.egg',
     '/usr/lib/python2.7/dist-packages',
     '/home/p3/github/pycaldav',
     '/usr/lib/python2.7',
     '/usr/lib/python2.7/plat-x86_64-linux-gnu',
     '/usr/lib/python2.7/lib-tk',
     '/usr/lib/python2.7/lib-old',
     '/usr/lib/python2.7/lib-dynload',
     '/usr/local/lib/python2.7/dist-packages',
     '/usr/lib/python2.7/dist-packages/PILcompat',
     '/usr/lib/python2.7/dist-packages/gst-0.10',
     '/usr/lib/python2.7/dist-packages/gtk-2.0',
     '/usr/lib/python2.7/dist-packages/ubuntu-sso-client',
     '/usr/local/lib/python2.7/dist-packages/IPython/extensions',
     '/home/p3/.ipython']
   

在google上搜到解决方法: <http://stackoverflow.com/questions/20327621/calling-ipython-from-a-virtualenv>, 在自己的系统上执行:

    python -c 'import IPython; IPython.terminal.ipapp.launch_new_instance()'

提示如下:

    (dev)p3@p3-ubuntu:~$ python -c 'import IPython; IPython.terminal.ipapp.launch_new_instance()'
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
    ImportError: No module named IPython


解决方法在虚拟环境下安装ipython:

    pip install ipython


然后再执行:

    python -c 'import IPython; IPython.terminal.ipapp.launch_new_instance()'


在系统的.bash_rc(mac os下是.bash_profile)配置一个别名

    alias ipy="python -c 'import IPython; IPython.terminal.ipapp.launch_new_instance()'"

这样设置后ipy使用的就是虚拟环境了

    
