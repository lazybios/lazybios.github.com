---
layout: post
title: netstat&lsof查看端口情况
categories:
- devops
tags:
- netstat
- lsof
- linux
---

### netstat
netstat用来查看系统当前系统网络状态信息，包括端口，连接情况等，常用方式如下：

`netstat -atunlp`,各参数含义如下:

+	-t : 指明显示TCP端口  
+	-u : 指明显示UDP端口
+	-l : 仅显示监听套接字(LISTEN状态的套接字)
+	-p : 显示进程标识符和程序名称，每一个套接字/端口都属于一个程序
+	-n : 不进行DNS解析
+	-a 显示所有连接的端口

执行后得表格一目了然，就不做截图了，当然，在众多表目中找一个特定得，肯定不那么顺手，一般该指令会遇grep配合使用，比如查找端口22,就用`netstat -tunlp | grep 22` 或者干脆`netstat -an | grep 22`就可以了，查看其它端口类似，当然也可以通过端口状态查找即`netstat -anp | grep TIME_WAIT`，即只会显示含有`TIME_WAIT`字符串的条目
	
### lsof
lsof的作用是列出当前系统打开文件(list open files)，不过通过`-i`参数也能查看端口的连接情况，-i后跟冒号端口可以查看指定端口信息，直接-i是系统当前所有打开的端口

`lsof -i:22 #查看22端口连接情况，默认为sshd端口`  如下图：

![查看连接数]({{site.IMG_PATH}}/2015-03-02-00.png)

可以看到当前通过端口22连接到机器的一共有4个(主机名和ip已打码)，通过该命令就能清楚知道当前端口状态


-完-