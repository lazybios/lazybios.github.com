---
layout: post
title: ubuntu平台源码安装nginx
categories: Linux开发
tags: 
- nginx
- linux
- ubuntu
---

通过源码编译安装nginx，简单的说下操作步骤，具体细节不写了，网上教程一大堆,如果不想麻烦，可以直接参考[ubuntu 中文wiki](http://wiki.ubuntu.org.cn/Nginx)里指南，用包管理工具安装
>  wget http://nginx.org/download/nginx-1.2.9.tar.gz #下载制定版本的源码   
tar zxf nginx-1.2.9.tar.gz    
> ./configure #configure 配置参数，从`./configure --help`获取帮助    
> make 
> make install

+ 安装zlib libirary时，误把zlib1g 打成了zliblg，这是个很容易犯的错误，注意看清
> `apt-get install zlib1g zlib1g-dev

zlib 库为ngnix提供gzip压缩功能

+ 安装openssl时，openssl development package的安装，并不像网上说的那样 `apt-get install openssl-dev`或`openssl-devel`,正确的是`apt-get install libssl-dev`
> `apt-get install openssl openssl-devel`

+ 安装时要确保安装目录（/usr/local/nginx）的访问权限，安装后目录结构
> 默认安装目录(/usr/local/nginx)   
程序安装目录 (/usr/local/nginx/sbin)    
配置文件目录(/usr/local/nginx/conf)

以上路径可以通过，调整`./configure`参数进行调整，具体可以进入到源码目录执行`./configure --help`查看帮助，或者参看[官方wiki](http://wiki.nginx.org)    
> 推荐 `./configure --prefix=/usr/local/nginx-x.x.x`来安装nginx，这样可以避免更新升级时对nginx的覆盖带来的意外   
--prefix 参数指定的就是nginx安装位置，一旦设定了该参数，其它如conf，sbin也会连接到该目录下, 即prefix/sbin,prefix/conf

> 手册mannal

另外最容易出错的地方就是权限和路径问题，如有错排错时多加留意下这方面问题

参考阅读:   
[nging wiki](http://wiki.nginx.org)    
[Nginx HTTP Server Second Edition](http://wiki.ubuntu.org.cn/Nginx)    
[ubuntu 中文 wiki]()
