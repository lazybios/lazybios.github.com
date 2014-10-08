---
layout: post
title: mysql中char,varchar,text之间的异同
categories:
- mysql	
tags:
- mysql
- linux
---

####char 
最大长度2在255**字符**（0~255字符），可以被最大存放255个字符，因为是固定大小存储，所以不考虑字节数，直接为字符。可设默认值，检索时会自动将尾部空格截断，除非`PAD_CHAR_TO_FULL_LENGTH`sql模式被开启。

####varchar 
变长字符，最大长度为65535**字节**（0~65535字节），其中1~3个字节用于存储长度，即varchar(n)中的n值，这里的单位是字节数，所以存储字符数，跟字符采用的编码格式有关系，utf-8中单个汉字占用3个字节，gbk占用2个字节，varchar(255)，能存储的汉字个数分别为utf-8 (255/3)个，gbk（255/2)个，且末尾的空格不会被截断。

####text
也是变长字符串，其后不能跟指定大小，即使指定了也会被忽略掉，同时text不能有默认值(default)，text会把65535全部用来存储数据，其使用自身长度之外的空间(2字节2^16=65526)存储数据值的真实大小


mysql所有集合都是PADSPACE的,即char,varchar与text中做值的比较都主动被截取掉末尾的空格，关键字`Like`除外

####三者的使用原则
char为固定存储长度，会浪费容量，检索效率char>varchar>text      
知道固定长度用char    
大小范围会变化的字段用 varchar    
超过255字节的只能用varchar或者text      
能用varchar的地方不用text       
最后其实如果 不是做产品优化仅原型开发时，可忽略该问题，全部使用varchar
####参考    
[The CHAR and VARCHAR Types](http://dev.mysql.com/doc/refman/5.0/en/char.html)    
[http://blog.csdn.net/wxq1987525/article/details/6564380](http://blog.csdn.net/wxq1987525/article/details/6564380)