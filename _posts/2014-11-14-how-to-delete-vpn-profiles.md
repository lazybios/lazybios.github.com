---
layout: post
title: 命令行删除无用vpn配置
categories:
- mac pro
tags:
- mac
- osx
---

修改完vpn配置后，机器以前旧的vpn设置就无效了，有的良心脚本会在生成配置时倒入到profiles中一分，但是有的没有生成profiles，这时如果手动删除20~30个配置 然后输入密码，估计会累成狗~


方法是使用 Terminal，也就是终端:

1.使用命令 `networksetup -listallnetworkservices`
上面命令会列出现有的网络服务名

2 使用 `networksetup -removenetworkservice networkservice_name`
**networkservice_name**是你要删除的服务名，如果含有空格什么的会无法识别到，先到偏好设置里面把服务名给改了。
这样就可以删除干净了，简单方便。
