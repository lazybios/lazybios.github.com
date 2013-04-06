---
layout: post
title: windows下安装配置git
categories:
- git
tags:
- git
- git bash
- 版本控制
---

重装了系统以后，git之流就全部被无情的删掉了，现在用到了就要再装一遍，苦逼啊...

#####步骤
+ Windows下安装，首先得下载并安装
> [最新版git下载](http://git-scm.com/downloads)    

如果你不介意安装目录那么就一路next就好，介意的话略微停顿下换个安装目录就好  

+ 设置git
> `git config --global user.name "Your Name Here"`   
> `git config --global user.email "your_email@example.com"`

+ 设置git bash显示中文
> `alias ls='ls --show-control-chars --color=auto'`
