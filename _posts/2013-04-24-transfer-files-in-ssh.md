---
layout: post
title: SSH协议下传输文件
categries:
- Linux
tags:
- ssh
- linux
---

最近来回于Linux与Windows间，在Linux下写好程序还需要在传回Windows，一般情况是搭建一个Ftpserver，但是我的Windows里装了不少软件了，想在不改变现有环境的基础上去完成这项工作，突然想到了用ssh协议，不知道可行否，Google了一下，还真被我找到了！linux安装lrzsz,使用sz和rz命令就可以了，具体做法:  

+ Linux下安装lrzsz 
> `sudo apt-get install lrzsz`   

+ lrzsz是建立在Zmodem文件传输协议上的传输文件工具，所以Windows端需要支持ZModem的telnet/ssh客户端(xshell\putty都支持)     
+ 最后使用rz、sz进行接受和传送文件，其有很多参数但如果只在Xshell下用的话直接使用 `rz filename` 或 `sz filename` 就可以了,Xshell在输入命令后自动会弹出一个对话框供你选择存放文件的路径，当然这些都是在你已经与Linux建立ssh通信的基础上

其实还个终极解决办法，那就是用U盘！哈哈...
