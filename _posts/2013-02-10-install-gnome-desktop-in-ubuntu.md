---
layout: post
title: Ubuntu 12.04下安装gnome桌面
categories:
- Linux
tags:
- Linux
- Ubuntu
---

今天Ubuntu下的Dash主页突然搜不到应用了，只能显示文本文件等源文件，网上找了下也没找到好的解释，好多人说法是重装就好了，好崩溃呀--!不过还好我重启了一下问题就解决了，倒是突然然我对gnome桌面产生了兴趣，于是做了下尝试

简单的调研了下，gnome是在linux中应用比较广泛的一种用户界面，因为自己是直接从12.04上手的，用的最多的也就是Unity，不多在其它发行版中页多少接触过一些，我选择安装的是精简版的gnome-session-fallback，让untiy和gnome在ubuntu下共存的方案

GNOME Session Fallback
> `sudo apt-get install gnome-session-fallback`

在ubuntu下有3种版本的gnome，其中上述是最精简的版本，仅仅安装gnome本身，只有30多MB，另一种是稍大的gnome shell，该版本会安装一些额外的应用程序，不过该版本包含GNOME 3，还有一种就是完整版，其不仅含有gnome本身也还有众多的应用，所以体积远远大于前2者，大概400多MB,适合完全从untiy迁移到gnome桌面的用户

GNOME SHELL
> `sudo apt-get install gnome-shell`

GNOME Desktop Environment
> `sudo apt-get isntall gnome`

这样就可以了，安装完毕以后就可以注销当前用户，在登录页点击户名上方的ubuntu图标，下拉菜单会显示所有可以使用的用户界面名称

> GNOME Classic   
GNOME Classic(No effects)      
二者区别是后者没有花哨的图形染效果，相比前者无染的后者会快一些

至此gnome安装完毕了，从此就可以在gnome和untiy下切换了


