---
layout: post
title: webpy下的session问题
categories:
- webpy
tags:
- session
---

最近用webpy写一个小项目，不得不说cookbook实在是太简略了，文档就是一个一个的小例程，基本上几个小时就能看完，但是实际操作的时候就又会蹦出一堆问题，从昨天到今天写一个简单的用户认证就碰到好多问题，感觉自己对python仅仅是了解知道的地步，距离说会还很遥远，相较其它语言python确实很容易上手，但正是因为它简单很容易让人误将一时的成功当作实际掌握，看来《笨办法学Python》还是有一定实际意义的。

#####webpy使用中遇到的问题
session会话的问题，webpy下session不能在debug模式开启下进行，官方虽然给出了解决方案，但不知道什么原因在我这里不起作用，后来查阅github上一些源码资源，发现很多人用webpy的processor处理器的钩子函数结合web.ctx.session进行管理session，具体方法:   
**code.py**
> `...`    
> `app =web.appllication(urls,globals())`     
> `#创建初始化session`    
> `session = web.session.Session(app, web.session.DiskStore('sessions'),initializer={'email':''})`    
> `#定义钩子函数session_hook`    
> `def session__hook():`      
> `>>>>web.ctx.session = session`   
> `app.add_processor(web.loadhook(session_hook))`   
> `...`

session使用同样要通过`web.ctx.session.email`来读取修改，造成session不能在debug下使用官方给出的原因是会重载入俩次，所以就会导致前面初始化后的被覆盖掉
> _注意！！！：session并不能在调试模式(Debug mode)下正常工作，这是因为session与调试模试下的重调用相冲突(有点类似firefox下著名的Firebug插件，使用Firebug插件分析网页时，会在火狐浏览器之外单独对该网页发起请求，所以相当于同时访问该网页两次)_

我在查阅资料的时发现一篇不错的讲解文章，[webpy猫腻之session with reloader]('http://www.cnblogs.com/Jerryshome/archive/2012/03/02/2377384.html')，该文章中用到的方法与cookbook相似，同时与本文这个方法异曲同工，都是在app启动之前做了手脚
