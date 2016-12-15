---
layout: post
title: "快捷登录SSH的Menubar小工具"
date: "2016-12-16☄"
---

不知道大家有没有使用menubar习惯，在Github上发现一款借助menubar快速登录远程服务器的小工具(Shuttle)。感觉很不错,拿来给大家安利一下。

### 安装
安装没啥可说的，跟其他软件一样，下载后，拖到`Applications`文件夹下打开就行了。

### 使用

![shutle]({{site.IMG_PATH}}/shutle.gif)

使用上也没啥特别需要说的，一图胜千言，况且还是个动图。唯一可能要注意的地方是，这个Shuttle的配置跟Sublime一样也是通过编辑`json`格式的配置文本生效的，在配置中你可以选择默认启动终端类型，设置是否开机自启，以及设置你要管理的服务器配置等，见下图。

shutle-1.png

![shutle-1]({{site.IMG_PATH}}/shutle-1.png)

轻量小巧用来管理自己的side project最好不过了，并且因为是文本个戳的配置文件，所以可以当做dotfiles来管理，这样即使是迁移机器也不担心了。


-完-

### 参考引用
+ [http://fitztrev.github.io/shuttle/]()