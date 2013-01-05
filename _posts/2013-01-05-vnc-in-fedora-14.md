---
layout: post
title: Fedora下的VNC服务配置
categories:
- Linux
tags:
- Fedora
- Linux
- VNC远程桌面
---

本来现在是复习时间，但奈何我越到这个时候思维越是活跃，各种想法是各种涌现啊，看书是只要不看课本什么书都看的效率奇高，晚上有捣鼓下linux，今天配置了一下fedora14下的VNC远程登录服务，记录一下吧

VNC(Virtual Network Computing),是linux上的远程桌面服务。分为客户端和服务端，VNC server会在服务端开启一个端口监听用户设定的端口，一般在5091～5010间，这个端口号要在客户端连接的时候告诉客户端，很重要！

####安装VNC服务器
>`yum install tigervnc-server`

####VNC指令说明
> `vncserver [:端口号]`
  
开启vnc服务，并监听 在509 + 10 × 端口号(端口号小于[1-10])

> `vncserver [-geometry 1024x768]`

设置登录默认分辨率,设置后要重启一下vnc服务 `service vncserver restart`

> `vncserver [-kill :端口号]`

将已启动的VNC端口删除，根据身份控制

首次启动服务会要求输入密码，密码不可显示，这个要记住，输出的密码文件会放到用户的主目录下，.vnc文件夹中，密码是加密过的

>`vncpasswd` 修改密码

####开发防火墙规则
> `sudo /sbin/iptables -I INPUT 1 -p TCP --dport 5901:5910 -j ACCEPT`
因为默认端口是从5091～5010，所以一次性开发11个，没准会用到哪个，这样就ok了

**PS:** VNC需要身份登录，必须在用户的主目录下初始化化有.vnc目录，才能登录，否则只能容许含有该文件的用户远程图形界面登录，并且VNC Server是独立提供给‘单一’客户端的，所以如果从重新开启一个连接就就要再开一个端口了








