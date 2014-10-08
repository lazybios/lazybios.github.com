---
layout: post
title: Python中创建多行字符串(Pythonic Way)
categories:
- Python	
tags:
- python
- pythonic
---

各位pythoner一定遇到过使用原生sql查询数据库的需求，有时候sql语言长到无法在同一行中放置，如果使用`\`,`+`或`'''...'''`又会意外的引入中间的字符(如\n与space)

用下面的方法可以很好的定义多行字符串

{% highlight python linenos %}

sql = ("this is a mutiple "
		"line,you can try print")    
print sql

{% endhighlight %}

结果自然是显示为单行，且不会有任何多余的字符