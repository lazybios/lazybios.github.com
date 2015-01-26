---
layout: post
title: flash[:notice]与flash.now[:notice]区别
categories:
- Rails
tags:
- ruby
---

Rails中通过flash可以在http跳转中方便的传递消息对象(该消息只能维持到下次http跳转),常用有两种

{% highlight ruby nos %}
flash[:notice] = "notice message"
redirecr_to other_path
{% endhighlight%}

但上面的调用方法只能在两个不同的actiion里通过调用`redirect_to`生效,想要在同一个action里传递，需要使用`flash.now`方法,见下

{% highlight ruby nos %}
flash.now[:notice] = "notice message"
render :other
{% endhighlight %}


注意使用`falsh.now`不能用`redirect_to`做跳转，否则falsh无法完成消息传递。


-完-
