---
layout: post
title: Google Python 风格指南(一)
categories:
- Python
tags:
- python
- code style
published: false
---

####背景
Python是google的主要脚本语言。这个风格指南是一个关于Python编程做与不做的参考，帮你正确规范你的代码。对于vim我们创建了一个[vim设置文件].对于Emacs，默认设置就足够了。


####imports 导包
只对packages和modules使用imports
#####定义
实现从一个模块到另一个模块的代码共享重用机制
#####好处
命名空间管理规则简单。每个代码的标识符通过一种固定的方式表示；`x.Obj`表明`Obj`对象定义在`x`模块中
#####坏处
模块名称仍然存在名称碰撞。一些模块的名称太长会带来不方便
#####实践
通过`import x`导入packages和模块   
通过`from x import y`表示x是package前缀和y是不定前缀的模块名称
使用`from x import y as z`避免同一个文件中导入两个相同模块名称或`y`一个冗长的模块名称

例如`sound.effects.echo`可以按下列方式导入
```python
from sound.effects import echo
...
echo.EchoFilter(input,output,delay=0.7,atten=4)
```

不要在imports语法中使用相关名称。即使用package的全名,即使对于同一package的模块。这样可以避免无意导入一个package两次。

####Exceptions 异常
异常是容许的但必须要谨慎使用
#####定义

 
