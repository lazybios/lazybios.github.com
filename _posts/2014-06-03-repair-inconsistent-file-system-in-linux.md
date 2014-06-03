---
layout: post
title: fsck修复文件系统不一致问题
categories:
- Linux
tags:
- fsck
- ubuntu
---


Ubuntu几次意外关机后，终于出问题开不了机了。开机后一直停在ubuntu logo那里，使用Resovery mode的resume查看了一下，错误提示    
> Buffer I/O error on device sda10,blabla...    
>/dev/sda10:unexpected inconsistency;run fsck manually(i.e,without -a or -p options)    
> mountall: fsck /home[1187]  ....blabla...

大概意思就是/home盘分区有问题,出现了数据不一致性问题，需要通过fsck(file system check)命令行进行修复

####解决办法
1. 使用ubuntu的livecd,进入命令行后    
2. 进入terminal,使用 `sudo fdisk -l`查看待恢复分区位置(device boot)    
3. 几下盘符号(本例/dev/sda10)，执行`fsck -C -f /dev/sda10`,然后按照提示一直y就好了    
4. 最后在通过`fsck -C /dev/sda10`看一下是否OK     

这里的关键就是fsck指令,fsck（file system check）用来检查和维护不一致的文件系统。若系统掉电或磁盘发生问题，可利用fsck命令对文件系统进行检查,其常用到的命令参数

1. -C 用一个直方图来显示目前的进度    
2. -f 强制检查 一般来说，如果fsck没有发现unclean标志，不会主动进入细化检查，如果要想强制fsck进入细化检查，需要加上-f标志    
3. -t 制定文件系统类型，不过现在大多linux系统可以自动识别所以不加也不会有问题    
4. -a 自动修复检查到的问题扇区，不需通过交互下达指令   

####特别注意事项
**执行fsck时，被检查的分区务必不可挂在到系统上！即是许可在卸载的状态下执行!**

lost+found，该目录就是当使用fsck检查文件系统后，若出现了问题时，有问题的数据会被放置到这个目录中。所以理论上这个目录不应该有任何数据，若系统自动产生数据在里面，那么就需要特别注意下该文件系统了

另外执行fsck指令，其实是在调用e2fsck,e2fsck有更多的辅助参数

####参考引用
[《鸟个的私房菜——基础学习篇》](http://vbird.dic.ksu.edu.tw/linux_basic/0230filesystem_3.php)

