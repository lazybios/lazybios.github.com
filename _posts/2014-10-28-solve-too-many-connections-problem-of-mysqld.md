---
layout: post
title: Mysql连接数过多解决方案
categories:
- mysql	
tags:
- linux
- mysql
---

报这种错误的原因是因为mysqld上的连接数目太多，超过了my.conf里`max_connections`变量设置的值。而连接数被一个进程过多占有一般是因为不合理的使用mysql连接导致的，比如使用mysql后使用完毕没有及时关闭，之后又有新的进程进行连接且没有复用之前的连接。

处理这个问题可以修改my.conf设置,如下:

```sh

[mysqld]   
set-variable=max_connections=500

```

当然如果出现这个问题，意味着你接下来得连接是不会成功的，所以只能是通过`kill -9`杀掉进程然后重新启动mysqld，此外还有个命令`SHOW FULL PROCESSLIST`可以查看当前连接数的详细情况，不过需要事先连接到mysqld上，所以如果你已经出现了报错，现在查看是于事无补的-.-,不过这个命令可以让你再重启后查看是哪些进程占用了所有的连接。

