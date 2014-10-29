---
layout: post
title: Js中3种声明全局变量的方法
categories:
- javascipt	
tags:
- js
- linux
---


1. 在function外通过`var a = "hello,global";`     
2. 隐式声明全局变量`a = "hello,global";`,可以在function内使用，执行后自动变为全局变量
3. `window.a = "hello,global";` 通常用于匿名函数执行后将函数公开到全局使用    

以上声明方式也会因为浏览器的不同，表现形式也会有所区别，比如在ie6~9中 从window对象中是无法取到第1，2种声明的全局变量的，并且删除变量的行为也会有所区别`var`声明的变量无法删除，其它两种方式会因为浏览器不同删除行为不同。


####参考  
[JavaScript声明全局变量三种方式的异同](http://www.cnblogs.com/snandy/archive/2011/03/19/1988284.html)
