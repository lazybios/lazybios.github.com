---
layout: post
title: Mysqldb的sql语句转义问题
categories:
- tornado	
tags:
- tornado
- python
- linux
---

在tornado web中用了torndb(torndb是对Mysqldb的封装)，在拼凑sql语句的时候遇到了单引号转义问题，具体解决方法如下

####方法一 
利用MySQLdb提供的转义功能，过滤内容    
`import MySQLdb`    
`input = MySQLdb.escape_string(input)`   


####方法二
利用MySQLdb中execute()接受多个参数使用去避开字符串导致的字符串拼接问题    
 + `cursor.execute("insert into resource(cid,name) values(%s, %s)" , (12,name) );`    
 MySQLdb会自动替你对字符串进行转义和加引号，不必再自己进行转义   
 + `cursor.execute("insert into resource(cid,name) values(%s, %s)" % (12,name) );`    
 利用python的字符串格式化自己生成一个query，也就是传给execute一个参数，此时必须自己对字符串转义和增加引号


