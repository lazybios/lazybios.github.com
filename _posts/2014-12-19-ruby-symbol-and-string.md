---
layout: post
title: Ruby：Symbol 与 String
categories:
- ruby
tags:
- rails
- ruby
---


接触rails时，ruby并不熟悉，ruby领域里一个明显的特点,框架的影响力大于语言本身,在学习rails框架，基本上可以屏蔽ruby语法细节直接上，大概几个demo下来就能掌握框架的使用，但是要进行更复杂功能的开发，还需要回到ruby语言，把那些语法细节搞懂,其中一个困扰我很久的问题就是Symbol与String的区别，因为在rails中有很多根据Symbol实现的部分，吊诡的是这些部分又可以通过字符串实现比如routes设置，下面罗列的是一些`:var`与`"var"`区别

+ 前者是symbol对象 后者是string对象，通过xx.class查看 二者直接转换 `String#to_sym、Fixnum#to_sym和String#intern`

+ 采用symbol做hash key 可以节省内存 因为对于string来说，即使是相同的字符串但它们确实不同得对象  
a.object_id 与 b.object_id 对比 所以当你

```ruby

h['ruby'].name = "ruby"
h['ruby'].author = "matz"
h['ruby'].brith = "1995"

```

这时对于"ruby"这个字符创建的是3个份，但是如果换成了h[:ruby]则仅仅在内存中又一份

+ 执行效率也比较高
不用多次动态生成"ruby"字符串，同时也免去了作为key值，不能随意改变，为了维护这一点，所以要对string对象施加保护，即类似加锁，但是对于symbol来说确实不必要的

+ ruby 的hash的key名可以是一个可变的对象,如下面代码
```ruby
l = [1,2]
h[l] = "somthing"
```

但是当l发生变化后要调用h.rehash 再hash才能通过 h[l]再次访问到原来得值

+ :var symbol是不可变

`:var = "hello"`这样的赋值语句会报语法错误

+ 通过symbol#to_i可以得到一个程序中唯一的整数，所以作为键名有天然的优势

+ 为什么Ruby runtime可以保证每一个symbol唯一？

因为Ruby把symbol存放在运行时维护的一个符号表里了，而这个符号表实际上是一个atom数据结构，其中存储着当前所有的程序级的name，确保不出现内容相同的多个对象，Python、Ruby则在运行时也保留这张表备用。这个表中存放的并不光是我们自己主动生成的symbols，还有Ruby解释器对当前程序进行词法分析、语法分析后存在其中的、当前程序的所有名字。这张表是Ruby引擎用的东西，通过使用Symbol，就让自己的对象跟Ruby引擎内部使用的对象成邻居了。所以String#intern这个方法叫做intern（内部化）。


参考：
http://blog.csdn.net/chunyang_guo/article/details/12021897
