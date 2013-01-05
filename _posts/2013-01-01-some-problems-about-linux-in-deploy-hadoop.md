---
layout: post
title: Hdoop部署中的linux操作记录
categories:
- Linux
tags:
- Linux
- Hadoop
---

今天在Ubuntu下部署了一遍Hadoop伪分布式，遇到一些问题，都是关于LINUX的。本来打算写的Hadoop部署相关的，看了下笔记本上那些问题，我想还把那些零散的问题总结归拢一下吧。

就按照问题出场次序来作为文章的脉络吧(我总觉的‘吧’这个字应该跟感叹号连用，但放到一起怕又显得排版突兀，遂又扯了这多没用的...囧)

###用户相关
#####为Hadoop创建专门新用户
> `useradd -d /home/username -s /bin/bash`

> + d 指定用户主目录 默认会将/etc/skel/下所有文件拷贝到用户主目录
+ s 指定用户默认shell
+ 没有在创建新用户时指定-p参数的，必须通过`passwd`指令设置密码后才能登录到系统


#####添加用户到sudoers，且执行时免密码输入
> `vim /etc/sudoers`

> + 在文件末尾追加`username ALL=(ALL) ALL`，之后可以使用 `sudo whoami`测试是否成功

#####shell alias别名应用
> `alias ll='ls -l --color'`

+ UID GID
> + UID 用户标识符 `/etc/passwd`
> + GID 用户组标识符 `/etc/group`

#####JDK安装
- 官网下载bin二进制文件解压 
- 修改文件权限 chmod
> + `chmod -a+x jdk-6u38-linux-i586.bin`
> + `./jdk-6u38-linux-i586.bin`执行后解压出jdk文件
- 修改环境变量 
> `vim /etc/environment` 追加：
>>- JAVA_HOME='存放jdk的路径'
>>- CLASSPATH='.'
>>- PATH = '....:$JAVA_HOME/bin'

+ `source /etc/environment`或`. /etc/environment`在当前bash环境下读取并执行FileName中的命令
>注意：source命令与shell scripts的区别是

>source在当前bash环境下执行命令，而scripts是启动一个子shell来执行命令。这样如果把设置环
境变量（或alias等等）的命令写进scripts中，就只会影响子shell,无法改变当前的BASH,所以通过文件（命令列）设置环境变量时，要用source 命令。

- 命令 export
> 将变量输出为环境变量，通过export -p 可以打印shell中已输出的环境变量


