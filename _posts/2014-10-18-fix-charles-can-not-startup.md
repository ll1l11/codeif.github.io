---
layout: post
title: 修复mac os x升级 10.10后不能启动Charles
---


mac os x升级到10.10后, 启动Charles报下面的错:

    dyld: lazy symbol binding failed: Symbol not found: _CGContextSetAllowsAcceleration
      Referenced from: /Library/Java/JavaVirtualMachines/1.6.0_31-b04-413.jdk/Contents/Libraries/libawt.jnilib
      Expected in: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/ApplicationServices
    
    dyld: Symbol not found: _CGContextSetAllowsAcceleration
      Referenced from: /Library/Java/JavaVirtualMachines/1.6.0_31-b04-413.jdk/Contents/Libraries/libawt.jnilib
      Expected in: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/ApplicationServices
    

安装官方新的java包后解决, 新的java包下载地址:
<http://support.apple.com/kb/DL1572?viewlocale=en_US&locale=en_US>
