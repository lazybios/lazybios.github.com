---
layout: post
title: rails实现返回页面
categories:
- rails
tags:
- rails
- ruby
---

在rails中实现返回上页的功能，有若干种方法，下面一一列举：

+ `request.env['HTTP_REFERER']` & `request.referer`

两种写法获取的结果一致，都是获取的HTTP请求头中的`referer`中的字段，即请求来源，之后再做跳转

+ `<%= link_to "Back", :back %>`
  

+ `redirect_to :back`

controller中的写法

还有在一些地方看到，`<%= link_to_function "返回上一页", "history.go(-1)" %> `这样的写法，但这里的`link_to_function`函数其实已经被弃用了，不过这种写法是可以的，即通过javascript中的浏览器对象，调用history方法，行为类似与点击浏览器的回退按钮


-完-