---
layout: post
title: mysql中null与not null的区别
categories:
- mysql	
tags:
- mysql
- linux
---

mysql 字段约束null，其实为一种规定约束，即在mysql中该**空值null是占用存储空间的** ,mysql默认字段约束为null

null 表示某个字段可以不含有任何值,可以为null    
not null 表示该字段必须含有某个值，且不能为null   

not null 表示不能向该字段插入`null`值，这里的空值null是特指mysql中的null，所以其实你是可以向该字段插入`''`这样的空串的(该空串仅在编程语言中表示空，在mysql中含义发生了变化)，同样null则表示可以接受字段值为null值


```sql
select * from table where col IS NOT NULL 
#判断是否为null值
select * from table where col <> ''
#判断是否为空串
```

上面关键是要把我`null`不等于`''`,二者的含义不同
