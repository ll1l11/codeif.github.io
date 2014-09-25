---
layout: post
title: python显示一个模块下的所有类
---

使用到了python的 inspect 模块

代码如下:

    # -*- coding: utf-8 -*-
    import sys
    import inspect
    
    import requests
    
    def main():
        model_moudle = sys.modules['requests']
        for name, obj in inspect.getmembers(model_moudle):
            if inspect.isclass(obj):
                print name, obj
    
    
    if __name__ == '__main__':
        main()

