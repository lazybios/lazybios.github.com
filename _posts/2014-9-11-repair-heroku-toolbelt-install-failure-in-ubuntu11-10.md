---
layout: post
title: CS169.1x中VM源导致的heroku-toolbelt安装问题
categories:
- Linux
tags:
- ubuntu
---

###背景
最近在上的一门国外公开课([UC BerkeleyX: CS169.1x 云计算与软件工程](http://www.xuetangx.com/courses/UC_BerkeleyX/CS169_1x_1/))，里面提供的虚拟机是Ubuntu11.10,在apt-get install 许多软件时总是遇到 `E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?`，许多源地址，访问返回总是报404错误

####解决方案
网上大概的搜了一下，发现说法不一，有的说网络不通的缘故，有的说源的问题，其实这种问题统一起来就是因为无法正确访问到url地址问题导致的，可以分别做排查，如果你的网访问站点没有问题，那么就可以排除是网络的问题，多半就是后者了，后者我也尝试过修改源为163镜像的11.10，可以解决部分问题，但是还会有一些其他错误，后边继续找到一个叫old-releadses的软件源[http://old-releases.ubuntu.com/ubuntu](http://old-
releases.ubuntu.com/ubuntu),修改source.list后发现，可以很好解决这个问题。

####修改Source.list步骤
1. sudo vim /etc/apt/source.list
2. 将source.list中的所有 http开头的url地址 替换为 `http://old-
releases.ubuntu.com/ubuntu` (替换以前记得做备份)
3. sudo apt-get update

上面三步就可以完美解决源导致的heroku-toolbelt安装失败问题，不过这只是第一个坑，后边还有heroku对sqllite3支持问题的坑需要注意 -.-
