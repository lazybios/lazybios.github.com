---
layout: post
title: 执行sudo时"command not found"解决方案
categories:
- rails
tags:
- rails
- ruby
---

使用sudo执行命令时，有的命令会提示"command not found",找了下原因是因为sudo在编译安装时候缺省使用了`—with-secure-path`参数，致使$PATH环境变量在sudo时被覆盖，于是就找不到要执行命令的位置了，自然会报错

###解决方法

通过使用别名(alias)，在执行sudo时同时定义环境变量为正确变量,添加下行到`~/.bashrc`

`alias sudo="sudo env PATH=$PATH"`

当然还有一个办法就是重装，取消`-–with-secure-path`参数,但个人觉得没必要，只要能保证正常行为就可以了,但是如果是多用户，那么就有必要了~ 


-完-