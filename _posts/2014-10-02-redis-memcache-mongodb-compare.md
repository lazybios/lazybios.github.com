---
layout: post
title: redis,memcache,mongodb三者比较
categories:
- linux	
tags:
- redis
- memcache
- mongodb
- linux
---

微信开发中需要用cache记录用户状态，于是找了下，大概有上面3个种，了解了一下，最后决定用redis了，三者的特性区别如下:

###Redis
+ 自带数据结构丰富除支持key-value外还支持set，hashmap，list等
+ 自带vm特性，可以突破物理内存限制，内存key值超过阀值会触发swap操作 
+ 自带持久化机制，可以定期将内存中的数据持久化到硬盘上，清零数据需要修改设置 
+ 单线程运行，存在IO瓶颈 
+ 全内存运行
+ 支持key值expire 
+ 可以做到一主多从，但是在一致性上还是不够成熟  
+ 容灾性较好，宕机后还可以从持久化中恢复过来      

###Memcache
+ 仅支持key-value方式，数据结构类型单一
+ 内存有物理内存上限限制
+ 重启memcache即清零key-value，清零方便，适合做缓存
+ memcached是多线程的，这样可以充分利用多核能力
+ 全内存运行 
+ 支持key值expire
+ 分布式比较成熟
+ 容灾性差，宕机后数据就丢了

###Mongodb
+ 更多的时候是被拿来与mysql做比较，作为关系数据库的一种替代 
+ 非关系型数据库，不用预先定义数据类型，有丰富的查询语句
+ 嵌套结构 不用使用表间连接查询，运行与内存中，查询效率高，适合海量数据存储 
+ 不支持事务
+ 部分数据在内存中，仅热点数据存放在内存中
+ 不支持expire


###参考
[http://250688049.blog.51cto.com/643101/1132097](http://250688049.blog.51cto.com/643101/1132097)   
[http://db-engines.com/en/system/Memcached%3BMongoDB%3BRedis](http://db-engines.com/en/system/Memcached%3BMongoDB%3BRedis)

