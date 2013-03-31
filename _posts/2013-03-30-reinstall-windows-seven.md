---
layout: post
title: 系统重装的善后
categories:
-杂记
tags:
-windows_7
---

重新装了下系统，电脑恢复了开始的整洁清新，感觉导致之前混乱状况的首要因素就是对下载的不加节制且不做分类管理，导致垃圾积累越来越多以致后来就完全不当成一回事儿了，貌似中是范这样的毛病，写博客也一样，积累了好多题材但都没写出来，有些本来但是做记录会是一份很好的资料，结果一拖延就流失了，以后要加强。

这次重装代价也不小，以前一年多积累的软件以及开发环境就给搞没了，现在还得从头一点一点的装，首先是几个基本的开发环境的安装恢复，本来是不愿意记录这些随处可Google到的东西，但是考虑到貌似我每次都得去google一下，还是以后看自己的笔记吧

####JDK安装

1. 下载正确版本的JDK,并安装到正确位置    
2. 配置环境变量java_home，path和classpath    
> JAVA_HOME: X:\Program Files\Java\jdk1.6.0  jdk安装路径   
> PATH: X:\Program Files\Java\jdk1.6.0\jre\bin java基本命令搜索路径   
> CLASSPATH: X:\Program Files\Java\jdk1.6.0\jre\lib 类的搜索路径
3. CMD中输入 java -version 显示正确的版本号即为安装成功

####Python安装
1. 官网下载正确的安装包,并安装到目标位置
2. 配置环境变量path
> PATH: D:\Program Files\Python27 python安装路径
3. CMD中输入python 可执行且显示正确的版本号即为安装成功

####Android开发环境
1. 官网developer处下载ADT捆绑插件，解压打开进入
2. 配置android模拟器存放路径ANDROID_SDK_HOME，其值可为任意路
3. 测试验证是否可用
4. 补充下载其他android sdk包

上面JDK及python安装没有什么变化，倒是android开发环境的安装便捷了不少，官方把之前的eclipse插件直接捆绑集成到一起了，但仅仅集成安装了4.2系统的sdk，如果需要开发其它版本下的软件，这需要继续补充下载其它相关sdk
