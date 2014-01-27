---
layout: post
title: ibus候选词不显示修正
categories:
- Linux日常
tags:
- linux
- ubuntu
---

我在ubuntu下用的是ibus双拼微软方案，网上有不少人说FCITX比较好，装了试了下，小巧，但是不知道什么原因调不出系统设置菜单调整双拼方案，找了写解决方案都没弄好，最后想想其实也没多大差别，本质上提升速度和输入体验还得靠自己的熟练度，与其这种事情上费心不如就用ubuntu默认自带的ibus。

但使用中还是不免有问题，就是候选词总是莫名的不显示，看来下解决方案就杀掉进程重启
> `killall ibus-daemon`    
> `ibus-daemon -d`



