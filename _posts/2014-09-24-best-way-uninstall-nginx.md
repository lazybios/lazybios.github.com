---
layout: post
title: Debian上完整卸载nginx并重装
categories:
- nginx
tags:
- linux
- nginx
---

####卸载

+ `sudo apt-get remove nginx`  # 可删除除/etc/nginx 配置文件外的所有文件

+ `sudo apt-get purge nginx` or `rm -rf /etc/nginx` #删除nginx配置文件

+ `sudo apt-get autoremove` #自动删除安装nginx时安装的依赖包


#####重装
`apt-get -o DPkg::options::=--force-confmiss --reinstall install nginx`    
主要解决了配置文件缺失，无法启动nginx问题   

