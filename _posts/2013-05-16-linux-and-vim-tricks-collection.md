---
layout: post
title: Linux与Vim技巧集锦[持续更新]
categories:
- linux
tags:
- vim
- linux
---

#####linux篇
######ssh [2013-05-16]
>`ssh -l username hostaddress`

######scp
>`[[user@]host1:]file1 ... [[user@]host2:]file2`   

######ubuntu root  
默认不开启root用户，所以安装时创建的用户即具有最高用户权限，可以通过`sudo`来执行超级用户权限，并可以`sudo passwd root`来修改root用户密码，进而`su`来切换到root用户状态

######Ctrl+z
bash环境中使用Ctrl+z可以让一个运行中的进程转到挂起到后台，进行其它操作，返回时可用`fg`命令

######0,1,2
0 stdin     
1 stdout  
2 stderror    
pipe
0 读端
1 写端

######\\[Enter]
\\[Enter]换行符，\与[Enter]间不能有任何其它字符，包括空格

######tee命令
这个命令是在，安装mongodb时遇到的，`cat filename |tee log`
功能：读取标准输入，然后将其输出到标准输出，并同时将其写入到制定文件

######linux修改ip地址
修改ip与掩码    
__即时生效__  `ifconfig eth0 192.168.1.105 netmask 255.255.255.0`    
修改网关    
__即时生效__  `route add default gw 192.168.1.1`    
修改dns  `vim /etc/resolv.conf`


---------------------------------
#####Vim篇
######跳到指定行 [2013-05-16]
编辑模式 ngg 或nG 跳到指定行   
命令模式 :n   
vim打开文件 `vim +n filename` 打开文件光标到n行   

######批量添加注释 [2013-05-19]
Ctrl+c进入可视化选择模式，通过`h j k l`选择要添加注释的行，然后按`I`(注意大小写)，在之后就可以随意在行首添加对应语言的注释符号了(ex:python #，c \\),添加后，再摁`Esc`退出，片刻就可以看到vim为选定的行自动添加了注释符

######撤消、重做
一般模式下，`u` 撤消 `Ctrl+r` 重做

######显示行号，tab,查询高亮[2014-01-16]
命令模式 `set nu`,`set list`,`set hls` 取消相应模式`set nonu`,`set nolist`,`set nohls`

######不同文件之间复制局部内容
`vim file_a`,`nyy+` 复制当前光标以后的n行，打开另一个文件，`vim file_b`,到指定复制位置`p`    
这个方法其实利用的是vim自带的寄存器，通过`:reg`可以访问到该区域

######vim内翻页
向上翻页 `ctrl+f`   
向下翻页 `ctrl+b`

######vim分栏
创建一个空白文件并上下分栏: `:new`   
将当前文件上下分栏显示 `:split`    
对应`:vnew`、`vsplit` ，为左右分栏，用`ctrl+w`配合`h,j,k,l`可在各栏间切换


