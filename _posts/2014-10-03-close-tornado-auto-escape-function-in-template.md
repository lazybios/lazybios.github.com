---
layout: post
title: 关闭tornado模板自动转义功能
categories:
- tornado	
tags:
- tornado
- python
- linux
---

Tornado Web的自带模板系统，在输出模板变量时默认是自动开启转义功能，这样可以使输出更加安全，但是在某些使用场景中却恰恰相反，希望无转义直接输出html源码，比如一些只读页面。Tornado为实现这个功能提供了两种方法:

方法一是在变量输出前先输入第一行，表明该变量无需转义，适合于有只读部分和用户读写部分的页面

`raw variable`     
`variable`

方法二 在模板第一行输入下面第一行，之后该页面就关闭了所有的转义功能，想要转义可以在局部使用第三行的语句

`autoescape None`   
`title`   
`escape(title)`
