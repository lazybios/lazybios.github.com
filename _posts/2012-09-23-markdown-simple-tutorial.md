---
layout: post
title: Markdown初步入门(一)
categories: [Blog & Jekyll]
tags: [博客,Jekyll]
---

和所有能搜到的Markdown的博客一样，本文首先也从Mardown是什么入手。

Markdown是一种源码与发布同样**易读易写**的一种轻量级的文本书写规则，学习Markdown没有学习HTML那么多的字母描述语言，基本都是一些简单容易记忆的符号语言，总之学习Markdown的成本很低，效率很高，基本上几个小时的阅读加练习就能掌握，但跟所有事物的学习一样要想让这样的利器融入到学习工作中，就必须勤加练习方能在使用中游刃有余。

###常用语法：

####标题

>"="或"-" 标识符分割
>插入"#"
>可以灵活的控制标题的大小与HTML中的h1～h6对应：

####强调：
 >单*或- 斜体
 >双*或- 粗体

####列表

>无序列表 '+'开头的
>有序列表 数字加'.'

####链接
行内 
>`[沸点](http://lazybios.github.com "Freshstu")`

参考:
>`[沸点][1]工作室

>[1]:http://lazybios.github.com "Freshstu"`

####图片
>行内
>>`![alt text](http://www.baidu.com/img/baidu_sylogo1.gif)`
>参考
>>`![alt text][1]

>>[1]:http://www.baidu.com/img/baidu_sylogo1.gif "百度logo"`
>结果
![alt text](http://www.baidu.com/img/baidu_sylogo1.gif "百度logo")

####代码
>通过一对"`"（ESC建下哪个键）标识 中间书写代码，分行要通过Enter

>格式化好的代码区块儿，每行都缩进4个空格或一个Tab，这样其中的特殊符号页都会转换为HTML实体而不会执行

###小结
>本文主要描述了在书写文章中的一些常用的符号标记，并且文中描述的一些内容只是众多方法中的一种或几种，本人认为在实现同一种效果中，只需要记忆一种便可，其余的只要见到了认识就好，所以在一些方法上做了一些删减。有想要更精美细致的排版可以参考下面网址
>#####参考网址
>>[Markdown语法详述](http://wowubuntu.com/markdown/index.html)
