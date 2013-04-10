---
layout: post
title: Python import机制
categories:
- python
tags:
- import
---

Python的import有俩中不同的语法表示，分别
> `import sys`   
 `from sys import path`    

这十分类似java中的package管理机制，这样做的目的是为了加强代码的模块化，从而利于管理较大型的代码项目

#####模块import
简单的说来`import sys`是将`sys`模块导入到了当前的.py文件的上下文环境了，但其`sys`内的成员，例如`path`变量还存在于`sys`的的上下文中，故如果要想引用打印`path`变量，必须要通过`print sys.path`才能正确访问该变量，否则如果在当前.py有定义`path`变量下则打印出来的是当前`path`变量而非`sys`下的，与之相反，`from sys import path`这种非整体模块导入的方式，直接将`sys.path`变量导入到了当前.py文件的上下文环境中，可以直接用`print path`打印，其已经跟`sys`无关了

上面描述可以通过自省函数dir()来观察 **dir()**函数是一个内建函数,用于查看指定作用域下可用的名字.在没有给dir()传入参数时返回当前文件上下文环境的可用名称

<script src="https://gist.github.com/lazybios/5354339.js"></script>

<script src="https://gist.github.com/lazybios/be0257b413ea0d8c7754.js"></script>

#####包import
包是若干个模块的集合，一个文件夹下创建空的\__init__.py文件，该文件即可被python解释器识别成一个包文件，在webpy项目下的子目录中的\__init__.py就是这个作用，其导入机制与模块相似，不同地方就是在导入模块时执行的是模块内的代码，但导入包时执行的是\__init__.py中的代码，并且包是可以嵌套，即多个较小的包可以聚合成一个较大的包，通过包这种结构，方便了类的管理和维护，也方便了用户的使用

可以使用`as`为导入的包进行重命名，一般在模块或报名的过长时使用
> `import sys as S`   
> `print S.path`

----------------------
#####import具体过程
Python中所有加载到内存的模块都放在sys.modules。当import一个模块时首先会在这个列表中查找是否已经加载了此模块，如果加载了则只是将模块的名字加入到正在调用import的模块的Local名字空间中。如果没有加载则从sys.path目录中按照模块名称查找模块文件，模块文件可以是py、pyc、pyd，找到后将模块载入内存，并加入到sys.modules中，并将名称导入到当前的Local字空间。    

可以看出了，一个模块不会重复载入 。多个不同的模块都可以用import引入同一个模块到自己的Local名字 空间，其实背后的PyModuleObject对象只有一个。
> + .py文件:Python源程序文件,文本文件   
+ .pyc文件:编译成字节码的python文件,可以使用python解释器,或者调用pycompile模块生成该文件. 
+ .pyd文件:pyd是python的动态链接库
+ .pyo文件:进行一定编译优化的后的字节码文件.另外,还可以控制python解释器,去掉”docstrings”,即代码中的无关文档字符串.

参考引用: [http://com1com4.iteye.com/blog/709980](http://com1com4.iteye.com/blog/709980)
