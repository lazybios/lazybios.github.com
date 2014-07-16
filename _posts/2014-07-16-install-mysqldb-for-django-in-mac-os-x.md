---
layout: post
title: Mac下mysql与mysql-python配置
categories:
- Django
- Python
tags:
- django
- mac
---

我的mac版本是 10.9.4 ，装mysql-python主要是为了在Django中使用，先装mysql，这个在mac下面装的确比较坑，查资料乱七八糟一大堆，幸而找了[此文](http://www.macminivault.com/mysql-mavericks/),该文中介绍了一个自动安装脚本，不仅帮你安装部署好mysql，同时也会帮你安装一款名为Sequel Pro的自动mysql自动管理工具，mysql安装好后的路径在/usr/local/mysql

#####注意
脚本会自动为你配置root用户的密码放置到桌面，这个不要搞丢了

下面是mysql-python
可以通过easy_install，pip这类工具安装，也可以自己手动编译安装，只是基本都会在最后import中碰到同样的问题

![bug显示信息]({{site.IMG_PATH}}/mysqldb_bug.png)

解决方案就是在库文件路径(/usr/lib)下建立一个软链接，如下命令：

> `$ sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib`

这样就可以正常的`import MySQLdb`了

#####手动安装注意
另外说一下手动编译安装中有个注意的地方，修改在下载好得源码包里的site.cfg，把注释掉的的mysql_config修改为本机mysql安装路径中得mysql_config文件(mysql_config = /usr/local/mysql/bin/mysql_config)，执行root权限执行下面两个命令    
> `$ sudo python setup.py build`      
> `$ sudo python setup.py install`      


#####参考链接
Mysql安装脚本说明：     
http://www.macminivault.com/mysql-mavericks/
mysql-python：  
https://pypi.python.org/pypi/MySQL-python/1.2.5  



