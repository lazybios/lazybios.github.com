---
layout: post
title: 解决vim打开windows文件乱码问题
categories:
- VIM
tags:
- linux
- vim
---

解决编码问题，关键是要搞清vim下的这三个参数:`encoding`,`fileencoding`,`fileencodings`,使得vim解码与文件编码相匹配

#####encoding
> vim内部使用的编码方式,通过在`~/.vimrc`中的`encoding`来设置，Vim在启动时读取`.vimrc`里的设置进行初始化，在使用中设置该参数意义不大，反而会引起其它问题

######推荐设置   
>`set encoding=utf-8`


#####fileencoding
> 如同名字叫的那样，文件的编码，`fileencoding`指的就是当前文件的编码格式，Vim在探测到文件编码后，会讲当前文件的编码方式设置到该变量中，通过`:set fileencoding`可以查当前文件编码格式,同样可以通过该变量转换文件编码格式，但是并不能通过该变量来纠正乱码，因为`fileencoding`这个变量是在Vim打开文件时自动探测赋值的

#####fileencodings
> 这个是上面变量的复数形式，顾其值应该是多值的，`fileencodings`可实现编码自动识别，VIM在打开文件后会依据该值的顺序一次尝试解码,直到成功为止！    
所以设置该值要根据编码的宽严度来排序，否则会出现大量的解码错误

######推荐配置
> `set fileencodings=ucs-bom,utf-8,cp936,gbk,big5,latin1`

基本通过在`.vimrc`里添加如下两行，基本解决大部分因为平台不同而导致的的乱码问题

[更详尽的参考在这里](http://edyfox.codecarver.org/html/vim_fileencodings_detection.html)

