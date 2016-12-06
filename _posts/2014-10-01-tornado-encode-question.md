---
layout: post
title: Tornado乱码问题解决
categories:
- Tronado	
tags:
- python
- tornado
---

使用tornado时，输出中文字符到控制台时报``UnicodeEncodeError: 'ascii' codec can't encode characters in position : ordinal not in range(128)`错误，看意思是Unicode编码和Ascii编码发生了冲突了，已经在文件的开头添加了`# coding:utf-8`的注释，但是在输出内容的时候还是给报错了，开始以为是mysql的编码格式不对，查看了一下`show create database db_name;` 或者进入到db中`use database name` 执行`status`也可查看编码,是utf-8没问题,那问题必然是出在python内部了,使用下面脚本查看了一下python默认编码方式,果然是ascii   

```python
import sys     
print sys.getdefaultencoding()   
```

这就解释了为什么会unicode与ascii编码有问题了，正是因为mysql是unicode才使得编码与ascii冲突，按照[之前那篇python编码问题文章]()的思路，在tornado代码开始加入下面语句，将ascii编码改为utf-8的就好了.

```python
import sys    
reload(sys)     
sys.setdefaultencoding('utf-8')    
```
