---
layout: post
title: torndb安装小记
categories:
- tornado	
tags:
- tornado
- python
- linux
---

torndb是从tornado中分离的一个子项目，用于与查询mysql数据库，其实就是在mysql-python上封装了一层而已，所以在安装torndb时，需要预先安装mysql-python模块，否则`import torndb`时会报`ImportError: No module named MySQLdb.constants`

安装mysql-python(`pip install mysql-python`)，运气好一次通过，如果你跟我一样使用debian，可能会遇到`pip install mysql-python fails with EnvironmentError: mysql_config not found`错误，不要紧只表示mysql_config没有被安装只需要安装一个叫`libmysqlclient-dev`的包

`apt-get install libmysqlclient-dev` 再重新`pip install mysql-python`就好了~

torndb本身文档只有一页，有问题直接读源码，含注释一共才264行，其实使用也相当简单，基本就是通过原生sql进行查询

###参考
[torndb 源码](https://github.com/bdarnell/torndb)   
[torndb 文档](http://torndb.readthedocs.org/en/latest/)
