---
layout: post
title: 关闭Dajngo的CSRF验证
categories:
- Django
tags:
- python
- django
---

用django封装了个直接push数据到graphite得接口，因为传得参数比较大超出了get传参所能承载的能力，只能用post，开始用python写了个post脚本，但老报403，headers也设置了，还是不行，后来直接用命令行`curl -d http://xxxxx`，还是提示403，仔细看了下才想起来django启动了csrf验证保护。

这样的话写脚本push会是个问题，服务端收不到数据~，想想干脆关掉csrf模块吧，但是发现单纯的到`settings.py`中注释`'django.middleware.csrf.CsrfViewMiddleware'`还是不能起到效果，另外如果这样的做的话，会彻底失去了csrf保护了，其实也不是个好办法，好的办法是能选择的对一些view中的发放进行放行。当然前提是你确保开放后不会遭受csrf攻击。

果然django已经自带了这个功能，具体如下操作:

{% highlight python linenos %}

from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def interface_view(request):
    pass

{% endhighlight %}