---
layout: post
title: Python使用cProfile pstats dot进行性能分析
---

## 用到的工具

  * cProfile, pstats 是python标准库 
  * gprof2dot 是一个开源python脚本， 在这里： <https://code.google.com/p/jrfonseca/wiki/Gprof2Dot>

## dist.py, 用于计算经纬度球面距离

    # -*- coding: utf-8 -*-
    from math import pi, sin, cos, acos
    
    def to_arc(d):
        return (pi * d) / 180.0
    
    def bearing(point1, point2):
        lon1, lat1 = point1
        lon2, lat2 = point2
    
        return sin(lat1)*sin(lat2) + cos(lat1)*cos(lat2)*cos(lon2-lon1)
    
    def bearing_to_dist(b):
        return acos(b) * 6378.137
        
    
    def get_distacne(point1, point2):
        point1 = map(to_arc, point1)
        point2 = map(to_arc, point2)
    
        return bearing_to_dist(bearing(point1, point2))
    
    if __name__ == '__main__':
        print get_distacne([0, 0], [0.11, 0.22])
    

## test2.py, 使用cProfile进行性能分析, 输出一个profile的二进制文件: out.pstat

    # -*- coding: utf-8 -*-
    import random
    from dist import get_distacne
    
    get_lat = lambda : random.uniform(-90.0, 90.0)
    get_lon = lambda : random.uniform(-180.0, 180.0)
    
    def foo():
        for x in range(1000):
            p1 = get_lon(), get_lat()
            p2 = get_lon(), get_lat()
            get_distacne(p1, p2)
    
    if __name__ == '__main__':
        import cProfile
        cProfile.run('foo()', 'out.pstats')
    

## 用 pstats 分析第一步中的profile文件，得到相关统计， out.pstats

    python test2.py
    ./gprof2dot.py -f pstats out.pstats > bitmap_desc.txt

## 使用dot命令,根据位图描述生成图片
    
    dot -Tpng -o out.png bitmap_desc.txt


在图片中每一个方块代表一个函数的执行

  * 第1行 所在模块:所在行:函数名 
  * 第2行 函数总计运行时间，含调用的函数运行时间(百分比) 
  * 第3行 函数总计运行时间，除去函数中调用的函数运行时间(百分比) 
  * 第4行 函数调用的次数 
