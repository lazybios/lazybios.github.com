---
layout: post
title: mysql命令行常用指令
categories: mysql
tags:
- mysql
- ubuntu
---

好久没有用mysql了，好多命令都模糊了，这回看tornado里的demo用涉及到这方面的，顺便会顾一下,只捡一些常用的记录下，其它什么的就用help查吧

1. 启动Mysql服务，shell命令: /etc/init.d/mysql start (stop,restart)   
2. 登录Mysql, mysql -uroot -p  
3. 显示所有数据库，show databases;  
4. 选择指定数据库，use database_name;   
5. 显示该数据库下所有表目，show tables;   
6. 创建数据库，create database database_name;   
7. 删除数据库，drop database database_name;   
8. 创建表，create table table_name(_id bigint(20),title,varchar(20));   
9. 删除表，drop table table_name;   
10. 增加表字段，alert table table_name add column field title...    
11. 删除表字段，alert table table_name frop field;
12. 查看表结构，show columns from table_name;


