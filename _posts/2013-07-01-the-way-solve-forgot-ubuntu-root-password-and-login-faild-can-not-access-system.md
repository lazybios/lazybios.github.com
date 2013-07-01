---
layout: post
title: 忘记root密码与登录失败的解决方案
categories: linux
tags: 
- root
- ubuntu 12.04
---

系统：ubuntu 12.04

前一次不幸误改了root密码，导致自己给忘记了，无法进入系统，后来通过grub的恢复模式进入文字操作界面，重新修改后才进入，这次又是因为修改/etc/enviroment 添加环境变量，导致无法进入系统，总是停留在登录界面，与忘记root密码一样，还是修改grub，重启后恢复到修改以前，既然已经俩次碰到这个问题，那就记录一下吧

1.启动计算机，进入linux的grub界面  
2.在恢复模式的选项上按“e”进入编辑模式，找到ro开始的地方，一直到最后，修改为`rw single init=/bin/bash`修改好后，按`Ctrl+X` 重启,登录后是root身份下的文字界面    
3.因为是root权限所以就可以做任意的设置更改了，譬如，忘记密码：执行passwd命令重建root密码; /etc/enviroment 错误，则可vim进入文件恢复到正常模式即可

