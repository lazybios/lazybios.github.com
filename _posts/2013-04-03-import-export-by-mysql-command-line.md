---
layout: post
title: mysql命令行下导入到处.sql文件
categories:
- Mysql
tags:
- Mysql
- 数据库
---

以前总是用phpmyadmin来管理mysql，很少到命令行下进行操作，系统重装后php环境也搞掉了，近期没有开发php打算，所以只装了一个mysql进行开发学习，虽然没有了界面操作，不过返璞归真到也高效

首先保证环境变量`PATH`的配置,我装的最新版5.6
> PATH X:\Program Files\MySQL\MySQL Server 5.6\bin   

#####基本操作
+ 命令行进入mysql
> mysql -h hostname -u username -p databsename   

hostname 本地为localhost 
-p后跟的是数据库名称，不是密码，密码会提示输入     

+ 显示所有数据库名称
> show databases;

不要忽略了分号
+ 创建数据库
> create table tablename;

+ 进入数据库
> use databasename;

+ 显示表信息
> describe tablename;

#####导入导出sql文件操作
+ 导入
> c:\>MySQL -h localhost -u root -p mydb2 < e:\MySQL\mydb2.sql   
> 也可直接进入到数据库中使用 `source e:\MySQL\mydb2.sql`

+ 导出
> c:\>MySQLdump -h localhost -u root -p mydb >e:\MySQL\mydb.sql 

使用了重定向符将文件导入到特定文件中




