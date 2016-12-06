---
layout: post
title: Devops马拉松30天——tcpdump Day.1
categories:
- Devops
tags:
- linux
- devops
---


tcpdump是一个抓包分析命令，工作在命令行界面，使用该命令可以抓取tcp/ip以及其他协议包，这种命令行形式的工具特别适用于没有图形界面的服务器管理上，并且可以基于tcpdump进行二次开发，定制全新符合自身应用场景的抓包分析软件。

####安装
一般默然类unix系统都是系统默认安装的，如果没有可以：

```sh

#centos
sudo yum install tcpdump	
#debian
sudo apt-get install tcpdump

``` 

####使用
tcpdump属于系统命令，需要在root状态下执行，使用前请确保权限的合法性
先上一张执行`tcpdump icmp`输出图

![tcpdump icmp]({{site.IMG_PATH}}/day1-0.png)

上图中`>`代表数据的流向，icmp为协议类型，length等blabla... 不要死记硬背，有了应用场景就自然记住了-.-

#####常用的参数

```sh
	-c #限制抓包的数量，默认无限
	-w #将输出存储为指定文件，`tcpdump -w xxx.cap` 然后再用更强大的分析工具分析
	-D #显示当前系统有哪些网络设备
	-i eth1 #只抓指定网卡流量 eth1可替换为任何有效设备名
	-vv #输出更加详细内容
	-n #不解析域名
	-A #以ASCII码形式显示抓包的内容
	-X #同时以16进制+ASCII形式显示包内容，类似wireshark
```

#####过滤分类
+ `tcpdump udp` 指定抓包协议类型，可以是ip,fddi(分布式光纤数据接口网络),arp,rarp,tcp,udp,icmp等等
+ `tcpdump port 80` 指定抓包端口号 此例为只抓取http协议端口
+ `tcpdump portrange 1-1024` 抓取指定端口范围内的包
+ `tcpdump src 127.0.0.1` 指定抓取本机发出取得包，即源网络为127.0.0.1的包
+ `tcpdump dst 192.168.1.1` 指定抓取目的地址为192.168.1.1的包
+ `tcpdump host 127.0.0.1` host的含义是无论发出还是获取，只要包含该主机地址，就抓取
+ `tcpdump greater 1024`  抓取大于1024字节的包 更具大小分类，同样的还有`tcpdump less 64` 抓取小于64kb的包,即异常流量

#####逻辑组合
可以根据以上的语法进一步的通过逻辑组合(and,or,not)，加大过滤强度

+ `tcpdump udp src 192.168.1.103 and port 8100`     
抓取满足 源地址为192.168.1.103 并且 端口号为8100的udp包
+ `tcpdump udp and src 192.168.1.103 or dst 192.168.1.104`   
抓取满足 源地址为192.168.1.103 或者 目标地址为192.168.1.104
+ `tcpdump tcp not port 22`    
抓取满足 端口地址 不是 22 的数据包

以上就是tcpdump最常用的几个参数，更详细的进阶请参考`man tcpdump`，写到这里应该算是结束了，但是应该在罗列一个应用场景，否则单纯性的记忆学习毫无意义-.-

####应用场景
可用于抓包调试socket网络应用程序，跟踪数据包的流向，判断socket是否畅通，最近我就调试了一把statsd接收日志异常的问题，很好的帮助我定位到了错误点~，如果你没有实际具体的应用场景，可以通过打开网页边浏览，边开终端以`tcpdump tcp port 80  -X`的方式查看具体变化，或者开两终端一个ping，一个`tcpdump icmp`，都可以看到实际的变化（如前面图片所示）


####参考资料
[tcpdump官网](http://www.tcpdump.org/)    
[tcpdump wikipedia](http://en.wikipedia.org/wiki/Tcpdump)    
[Linux tcpdump命令详解](http://www.cnblogs.com/ggjucheng/archive/2012/01/14/2322659.html)
