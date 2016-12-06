---
layout: post
title: 《The hard way learn ruby》(一)
categories:
- Ruby
tags:
- ruby
---

相信有不少同学跟我一样，对Ruby的学习是从Rails入手的，虽然根据其它语言的使用经验可以用Rails完成一些事情，但是读一些rubyist的代码后就会发现其中的差距。ruby知识体系零散，缺乏提纲挈领的梳理。于是我选择从头看《the hard way learn ruby》这份教材,一步一步把语言框架熟悉一遍，之后再回头挖文档,下面是前十五章的一些自认为有用的小梳理

#### 双引号与单引号的区别
双引号中`#{}`会被其中包含的对应变量替换，但是单引号会忽略其中的任何变量，只做为字符串来处理

#### `ARGV`与`gets.chomp`区别
二者的区别在于从何处获取输入，`ARGV`是通过命令行的形式进行传递的，即在程序执行前传入，而`gets`这种形式是通过读入标准IO(键盘)，在程序执行中获取外界输入。

#### `print`与`puts`区别
前者会自动在末尾追加`\n`换行符，后者需要手动进行添加

#### 同一文件是否可以在程序中打开多次
ruby中不会对文件打开次数进行限制

#### 格式化字符串时应该如何在`#{}`与`%{}`取舍
大多数的情况可以用`#{}`进行格式化字符串，但是当在字符串中有多个相同的变量需要替换的时候，可以通过`%{}`的形式，减少`#{}`的重复输入

```ruby
formatter = "something need to repeat more then one times %{name} and %{name} "
puts formatter %{name: 'nothing'}
#something need to repeat more then one times nothing and nothing
```


-完-
