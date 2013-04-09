---
layout: post
title: windows下ulipad安装相关
categories:
- Python
tags:
- ulipad
- 软件冲突
---

Python环境搭建好了，可没有个像样的编辑器，官方的IDLE太挫了，没什么心情用，网上溜达发现了Eclipse+pyDev和Ulipad这款国人自己开发的Python编辑器，看评论貌似还是比较靠谱的，因为现在只是写一些简单的东西和看别人代码，以学习模仿别人为主，所以主要还是看重编辑功能，对于项目的管理其实这个阶段不是很需要，所以就选择了后者，不过话说回来，Eclipse+pyDev的debug功能真心不错，以后弄复杂的东西可以一试它，下面说一下关于ulipad安装配置以及使用中碰到的问题
#####安装
下载清单
> Python 开发环境　[下载地址](http://www.python.org/download/)  
> wxPython unicode (根据windows版本及python环境选择下载) [下载地址](http://www.wxpython.org/download.php#stable)   
> comtypes模块安装 [下载地址](http://sourceforge.net/projects/comtypes/)   
> Ulipad 主角 [下载地址](http://code.google.com/p/ulipad/downloads/list)

按照上述顺序下载安装配置程序即可，因为ulipad是基于wxPython开发的，所以务必确认自己已经正确安装了wxPython这个模块，且版本也没问题
######*特别注意*
> 目前发现ulipad与**网易有道词典**相冲突，会出现先开启有道，再打开ulipad，没有任何响应，且ulipad安装目录下的error.txt内也是空白，遇到该问题尝试**先开ulipad，再打开有道词典即可**，特别是那些将有道词典开机自启动的童鞋们要注意呀

#####配置
打开ulipad，选择菜单->编辑->Python->设置Python解释器->增加 输入解释器路径 可以手动选择Python安装路径下的Python.exe文件 描述设置为2.7 console，再增加个Python文件下的Pythonw.exe 描述为2.7 window，至此就可以在ulipad下运行调试python程序了

最后要说一下的就是ulipad安装路径下的error.txt,如果遇到任何问题可以先查看下错误日志排错，如果遇到error.txt空白的情况，多半是像上文那样软件冲突了，关掉正在运行的可疑程序试一试








