---
layout: post
title: 利用django URL规则保持view逻辑一致性
categories:
- Django
tags:
- python
- django
---

一个莫名其妙的需求:`product\` 与`product\1`想要共存。看了下，貌似没找到在url.py中定义默认值的方法，于是想了想，利用django url规则解析顺序的特性，将弱匹配放到前面，强匹配`product\(?P<id>\d+)`放到后面，然后让两条路径都同时指向一个view中的方法,其中view中对应的方法id设置一个默认值，这样不会破坏view中的逻辑，但在url上也满足了`product\`与`product\1`共存的需求,其实在一些其它场合也能用到这样的思路~ 



```python
#view.py
 def product(request,id=34):
 	pass

#url.py
url(r'^product/$',views.product,name='product1'),    
url(r'^product/(?P<id>\d+)$',views.product,name='product'),

```




