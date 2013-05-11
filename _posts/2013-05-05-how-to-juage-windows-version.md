---
laytout: post
title: 编程判断Windows的不同发行版
categories:
- python
tags
- Windows Version
---

WinXp与Win7属于不同系列，在许多方面有着不同的配置，编程时需要针对相应特定平台作出不同的设置，故对于win版本的判断是很中重要的。
在Win的内部有很多判断方式，其中最简便的是cmd下使用`ver`命令。
> `ver`    
> `Microsoft Windows [版本 6.1.7600]`

该命令显示了win的内部版本号，这个版本号不同于系统的命名，windows命名自85年发布以来有很多变化，但内部版本好却是一脉相承的，见下表
> Windows 8                 --->    Windows NT 6.2.9200    
Windows 7 Series            --->    Windows NT 6.1.7600  
Windows Server 2008 R2      --->    Windows NT 6.1   
Windows Server 2008         --->    Windows NT 6.0   
Windows Vista               --->    Windows NT 6.0.6000   
Windows Server 2003 R2      --->    Windows NT 5.2   
Windows Server 2003         --->    Windows NT 5.2   
Windows XP                  --->    Windows NT 5.1   
Windows 2000                --->    Windows NT 5.0   
Windows Me                  --->    Windows NT 4.9   
Windows 98 SE               --->    Windows NT 4.1   
Windows 95                  --->    Windows NT 4.0  
Windows 98                  --->    Windows NT 4.0   
Windows 9x                  --->    Windows NT 4.0  
Windows 3.1                 --->    Windows 3.1   
Windows 3.0                 --->    Windows 3.0  
Windows 2.0                 --->    Windows 2.0   
Windows 1.0                 --->    Windows 1.0   

从上表也可以看出，Windows在发行时有很多版本虽然命名不同但同属一个系列，一般来说属于同一系列的发行版其设置方面基本相同(例：文件目录结构)，所以在编程时可以简单忽略认为是相同平台，这样在判断版本时就可以通过内部版本号对Windows系列做出区分，下面用python做简单判断
> `import platform`   
> `if platform.version()[0] == '6':`   

当然python对win的判断有很多方法，如sys模块及其它针对windows平台编程的模块，甚至可以通过os.system()执行cmd命令获取。
