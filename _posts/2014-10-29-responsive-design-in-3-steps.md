---
layout: post
title: 3步完成响应式设计
categories:
- frontend	
tags:
- responsive design
- 响应式
---


在当前响应式设计对于开发无疑是件大事儿，如果你还不了解响应式设计，可以查看我最近贴出来得关于[响应式网站]清单。对于新手，响应式设计听起来似乎有些复杂，事实上它远比你想象的要简单。我写了这个快速教程，帮助你迅速理解响应式设计。我保证你可以在3步内理解基本的响应式设计逻辑以及`media query`概念(已假设你已经掌握了css得基本知识)

####Step1. Meta标签(查看[Demo](http://webdesignerwall.com/demo/responsive-design/index.html))
大多数手机浏览器通过`viewport`宽度缩放html页面去适应手机屏幕。你可以使用`viewport`标签去重置这个值。下面关于viewport的代码告诉浏览器viewport宽度值设置为设备宽度并且禁用了初始缩放,将这个meta标签放置到<head>里

{% highlight html linenos %}

<meta name="viewport" content="width=device-width, initial-scale=1.0">

{% endhighlight %}

IE8以及以下版本不支持media query。你可以引入[media-queries.js](http://code.google.com/p/css3-mediaqueries-js/)或[respond.js](https://github.com/scottjehl/Respond)去使它们变得可以支持media query。

{% highlight html linenos %} 

<!--[if lt IE 9]>
	<script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
<![endif]-->

{% endhighlight %}


####Step2. HTML结构

在我们的例子中，我们有一个具有header，content，sidebar以及footer的简单页面。header具有固定的高度180px,content容器宽为600px,sidebar宽为300px。

![page-structure]({{site.IMG_PATH}}/page-structure.png)


####Step3. Media queries
[CSS3 media query](http://webdesignerwall.com/tutorials/css3-media-queries)是响应式设计的一个小技巧。它有点类似写if else语句，根据指定的`viewport`宽度告诉浏览器如何渲染一个页面根据   

下面的一组规则将会在`viewport`宽度在980px或更小的时候生效。基本上，我把所有容器的宽度单位从像素(px)设置为百分比值，以便让页面成为流动布局。


![980px-or-less]({{site.IMG_PATH}}/980px-or-less.png)

下面的是当`viewport`<=700px时生效，指定了#content和#sidebar宽度为auto并删除了float属性，以使它们的宽度可以充满显示页面

![700px-or-less]({{site.IMG_PATH}}/700px-or-less.png)

对于<=480px(手机屏幕),重置了#header的高度为auto,改变了h1的字体大小到24px并且隐藏了#sidebar

![480px-or-less]({{site.IMG_PATH}}/480px-or-less.png)


只要你喜欢，你可以写更多的media query语句。我仅仅在这个demo中用到3个media query语句。media query的目的是根据不同的`viewport`宽度，使用不同的CSS规则以达到不同的布局。media query 语句可以被放置在不同的文件中，也可以放置在同一个样式表中。

####结论
这个教程的目的是向你展示基本的响应式设计。如果你想要更深入一些的教程，可以查阅我之前的另一个教程[ Responsive Design With Media Queries](http://webdesignerwall.com/tutorials/responsive-design-with-css3-media-queries)(ps：或者看我的翻译[使用Media Query实现响应式设计]())
