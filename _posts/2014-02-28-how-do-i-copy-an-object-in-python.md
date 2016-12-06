---
layout: post
title: 如何在python中拷贝一个对象
categories:
- Python
tags:
- python
- python faq
- python 面试
---


对于一般案例可以用`copy.copy`或者`copy.deepcopy`，但并不是所有对象可以被拷贝。

```python
import copy
newobj = copy.copy(oldobj)
newobj = copy.deepcopy(oldobj) #递归copy
```

一些对象有自己到copy方法,比如字典类型有`copy`方法：     
`newobj = olddict.copy()`

序列可以通过切片来复制:    
new_list = L[:]

也可以使用`list,tuple,dict,set`函数拷贝对应类型对象，或相互之间进行类型间的转换

```python
#copy
new_list = list(L) 
new_dict = dict(olddict)
#covert
new_set = set(L) #list to set
new_tuple = tuple(L) #list to tuple
```

[原文链接](http://effbot.org/pyfaq/how-do-i-copy-an-object-in-python.htm)


